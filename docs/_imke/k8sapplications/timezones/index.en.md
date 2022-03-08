---
title: Time Zone Management
lang: en
permalink: /imke/k8sapplications/timezones/
nav_order: 7400
parent: Kubernetes Applications
---

In case your application requires time zone switching you can follow this guide.

## Prerequisites

To successfully finish this guide, you need the following items.

* `kubectl` [latest version](https://kubernetes.io/de/docs/tasks/tools/install-kubectl/)
* A running Kubernetes Cluster, created with iMKE, with a ready Machine Deployment.
  * See also [Creating a Cluster](/imke/clusterlifecycle/creatingacluster/)
* A valid `kubeconfig` for your cluster
  * See also [Connecting to a Cluster](/imke/accessmanagement/connectingtoacluster/)


## Time zones in Kubernetes environment

### Default behaviour in Kubernetes cluster

Kubernetes clusters inherit the time zone configuration from worker node, so that the default time zone in the Kubernetes cluster is the same as the one in your worker node - it's controlled by the kernel.

### Pods & Containers

While it is not trivial to change the time zone at the cluster level, there is an easy way to achieve this at pod and container level.

To achieve this goal, you can follow the scenario below:

#### Scenario

##### Start a test container

Start a minimal container e.g. a one with busybox. For example:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "100000"
```

Save this definition file as `busybox.yaml` and apply it to the cluster:
```bash
$ kubectl apply -f busybox.yaml
```

Wait a moment until the pod deployment has been finished and query the time from the container:
```bash
$ kubectl exec busybox-sleep -it -- date
```
You would get a similar output, in our example the time zone is `UTC`:
```bash
Wed Feb 26 10:57:53 UTC 2020
```

##### Change the time zone in the test container

To change the time zone at pod or container level, do the following:

Share your desired 'zoneinfo' file with the pod or container by mounting it from the worker node into the container. The following example shows the change to Berlin local time:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox-sleep-2
spec:
  containers:
  - name: busybox
    image: busybox
    args:
    - sleep
    - "100000"
    volumeMounts:
    - name: timezone-config
      mountPath: /etc/localtime
  volumes:
    - name: timezone-config
      hostPath:
        path: /usr/share/zoneinfo/Europe/Berlin
```

Basically, it's the same definition, file but with a volume named `timezone-config` which set the container's `localtime` to your time zone.

Save this definition file as `busybox-2.yaml` and apply it to the cluster:
```bash
$ kubectl apply -f busybox-2.yaml
```

After waiting a moment until the deployment has been finished query the time from the container:
```bash
$ kubectl exec busybox-sleep-2 -it -- date
```
You should get an output that shows the Berlin local time:
```bash
Wed Feb 26 11:01:04 CET 2020
```

## Summary

We learned the following:

* How to query time from inside a container
* How to mount `zoneinfo` / `localtime` to the container, so that the time zone changes.

Congratulations! You now know all required steps to change time zone in Kubernetes pods and containers.
