---
title: Multiple Availability Zones
lang: "en"
permalink: /gks/machinedeployments/multipleavailabilityzones/
nav_order: 6600
parent: Machine Deployments
---
# Multiple Availability Zones

## Introduction

A machine deployment is always bound to one availability zone (in short AZ)
as it's machines cannot exist in more than one (and machines from different
AZs are not mixed in one machine deployment). This restricts the storage
that can used by the pods running on these machines. So the storage that pods
running on these machines want to use need to be available in the same
availability zone the machine resides in.

## Single-AZ Setups

Having only one machine-deployment running (which is the default) poses no
problems as the storage for a pod is requested for the same AZ. If a pod
gets evicted to a different machine (for whatever reason) it will still
be in the same AZ and thus will be able to access that storage in the
usual way.


## Multi-AZ Setups

### Stay in the same AZ

If you create more than one machine deployment (like adding an additional
one to the default one), you can choose from different availability zones.
You are not required to use the same one you used for the first (default)
machine deployment (although you could if it suites your use-case). When
adding a second machine deployment in another AZ you need to be aware that
the kubernetes scheduler could decide to schedule a pod which was previously
running on a machine in one AZ to now run on a machine in a different AZ.
This will leave the pod in pending state unable to be run correctly as it
is missing it's storage which is anchored to the old AZ and cannot be
moved or migrated automatically.

A solution to this scenario would be to pin the pod to a specific AZ. This
can be done with a kubernetes feature called affinity, which can be specified
on either a deployment, statefulset or daemonset. With affinity you can
specify that a pod should only run in a specific AZ. This would make
sure that if a pod gets evicted from a machine it will only ever be
scheduled on a machine in that specific AZ. Here is an example for a
deployment with one pod in AZ ES1:

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: myapp
  name: myapp
  namespace: default
spec:
  replicas: 1
  ...
  template:    
    ...
    spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: topology.kubernetes.io/zone:
                operator: In
                values:
                - es1
      ...
```
(The three consecutive dots meaning we are omitting lines which are
 not vital to the argument we are trying to make ... the example
 will not work without the omitted lines though!)

### spread across multiple AZs for high-availability

There is a way to achieve higher availability by creating more pods
and use (anti-)affinity to make sure two pods of the same application
ido not run in the same AZ. As this whole topic is about storage we
need to choose a StatefulSet for this, otherwise it will not be
possible for the kubernetes scheduler to connect the pods with
it's respective storages (persistent volumes and their claims).

Here is an example for affinity rules of a StatefulSet with two
replicas, and demanding the two replicas not to be deployed in
the same AZ:
```bash
apiVersion: apps/v1
kind: StatefulSet
metadata:
  labels:
    app: myapp
  name: myapp
  namespace: default
spec:
  replicas: 2
  ...
  template:    
    ...
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: app
                operator: In
                values:
                - myapp
            topologyKey: topology.kubernetes.io/zone
      ...
```
(Again the three consecutive dots meaning we are omitting lines which are
 not vital to the argument we are trying to make ... the example
 will not work without the omitted lines though!)

For more complicated setup please consult the original kubernetes
documentation.


## Learn More

* [simple Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)
* [more complicated Affinity](https://kubernetes.io/docs/concepts/scheduling-eviction/topology-spread-constraints/)
