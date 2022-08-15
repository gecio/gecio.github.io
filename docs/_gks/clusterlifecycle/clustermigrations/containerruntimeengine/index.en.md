---
title: Migration Container Runtime Engine
lang: en
permalink: /gks/clusterlifecycle/clustermigrations/containerruntimeengine/
nav_order: 5000
parent: Cluster Lifecycle
has_children: false
---
# Migration Container Runtime Engine

## General Container Runtime Information

For a long time now Docker (or rather the dockershim to be more precise) has
been used as the default Container Runtime Engine in the underlying
infrastructure of Kubernetes. But maintaining this dockershim on the
development side has become a heavy burden on the Kubernetes maintainers.
To reduce this burden the CRI standard has been implemented, but unfortunately
Docker itself does not implement this standard (hence the dockershim!).

With the Kubernetes v1.20 release the deprecation of the dockershim has been
announced, scheduled for the release of v1.24. So to be able to upgrade to
Kubernetes v1.24 we will need the Kubernetes nodes to run a different
container runtime engine which is CRI-standard compatible. For GKS this
will be containerd.

## Changing the Container Runtime Engine Config

To upgrade from Docker to containerd the following steps are needed:

1. Edit the cluster configuration.
![edit-cluster-config](../../images/MigContRun01.png)

1. Change the value of the *Container Runtime* field from *docker* to *containerd* and save the changes.

![switch-cre-config](../../images/MigContRun02.png)

## Activating the Config Change

For the change to take effect, you need to rotate your worker nodes once.
This can be done by either upgrading to a new Kubernetes version or by
doing a restart of the rollout of your MachineDeployment. While the prior
can be done in the Web UI easily (and is already covered by [here](/gks/clusterlifecycle/upgradingacluster/)),
the latter will be shown below.

1. Check which container runtime your workers are currently using:

   ```bash
   $ kubectl describe node  | grep "Container Runtime Version"
   Container Runtime Version:  docker://19.3.15
   Container Runtime Version:  docker://19.3.15
   Container Runtime Version:  docker://19.3.15
   ```

   Here we see in the output that docker is still used as the container runtime.
1. Restart the Machine Deployment as follows:
    1. Click on the Machine Deployment of the cluster.
![choose-machinedeployment](../../images/MigContRun03.png)

    1. Click on the Restart button.
![click-on-restart-button](../../images/MigContRun04.png)

    1. Confirm the restart of the Machine Deployment.

 ![confirm-restart](../../images/MigContRun05.png)

   Now one after another, a new machine will be added and an old machine will be removed after its workload has been transferred into the rest of the cluster. After the last old machine has been removed, the restart of the
   MachineDeployment is complete.
2. Now check again, which container runtime powers your worker nodes:

   ```bash
   $ kubectl describe node  | grep "Container Runtime Version"
   Container Runtime Version:  containerd://1.5.4
   Container Runtime Version:  containerd://1.5.4
   Container Runtime Version:  containerd://1.5.4
   ```

This concludes the migration of your Kubernetes clusters container runtime
from *docker* to *containerd*.

## Learn More

More background information on the container runtime engines for Kubernetes is available here:

* <https://kubernetes.io/blog/2020/12/02/dockershim-faq/>
* <https://kubernetes.io/blog/2020/12/02/dont-panic-kubernetes-and-docker/>
