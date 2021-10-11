---
title: Migration Container Runtime Engine
lang: en
permalink: /imke/clusterlifecycle/clustermigrations/containerruntimeengine
nav_order: 5000
parent: Cluster Lifecycle
has_children: false
---

## General Container Runtime Information

For a long time now docker (or rather the dockershim to be more precise) has
been used as the default Container Runtime Engine in the underlying
infrastructure of kubernetes. But maintaining this dockershim on the
development side has become a heaven burden on the Kubernetes maintainers.
To reduce this burden the CRI standard has been implemented, but unfortunately
docker itself does not implement this standard (hence the dockershim!).

With the Kubernetes v1.20 release the deprecation of the dockershim has been
announced, scheduled for the release of v1.24. So to be able to upgrade to
Kubernetes v1.24 we will need the kubernetes nodes to run a different
container runtime engine which is CRI-standard compatible. For iMKE this
will be containerd.

## Changing the Container Runtime Engine Config

To upgrade from docker to containerd the following steps are needed:

1. edit the cluster configuration

   ![edit-cluster-config](edit-cluster.png)

1. change the value of the *Container Runtime* field from *docker* to *containerd* and save the changes

   ![switch-cre-config](switch-cre.png)


## Activating the Config Change

For the change to take effect you need to rotate your worker nodes once.
This can be done by either upgrading to a new kubernetes version or by
doing a restart of the rollout of your MachineDeployment. While the prior
can be done in the webUI easily (and is already covered by [here](/imke/clusterlifecycle/upgradingacluster/)),
the latter will be shown below.

1. check which container runtime your workers are currently using:
   ```bash
   $ kubectl describe node  | grep "Container Runtime Version"
   Container Runtime Version:  docker://19.3.15
   Container Runtime Version:  docker://19.3.15
   Container Runtime Version:  docker://19.3.15
   ```
   Here we see in the output that docker is still used as the container runtime.
1. restart the MachineDeployment by
    1. clicking on the MachineDeployment of the cluster

       ![choose-machinedeployment](choose-machinedeployment.png)

    1. clicking on the restart button

       ![click-on-restart-button](click-on-restart-button.png)

    1. confirming the restart of the MachineDeployment

       ![confirm-restart](confirm-restart.png)

   Now one after another a new machine will be added and an old machine will
   be removed after it's workload has been transferred into the rest of the
   cluster. After the last old machine has been removed the restart of the
   MachineDeployment is complete.
1. now to check again which container runtime powers your worker nodes now:
   ```bash
   $ kubectl describe node  | grep "Container Runtime Version"
   Container Runtime Version:  containerd://1.5.4
   Container Runtime Version:  containerd://1.5.4
   Container Runtime Version:  containerd://1.5.4
   ```

This concludes the migration of your kubernetes clusters container runtime
from *docker* to *containerd*.

## Learn More

More background information on the container runtime engines for kubernetes can be
found here:

* <https://kubernetes.io/blog/2020/12/02/dockershim-faq/>
* <https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/>
