---
title: StorageClass Setup
lang: en
permalink: /imke/k8sapplications/storageclasses/
nav_order: 7500
parent: Kubernetes Applications
---

We are providing one default storage class per Cluster.  
> __Caution:__
> This is managed by iMKE and can be **overwritten at any time**. Please create a separate storage class for your changes.

```
kubectl get storageclasses.storage.k8s.io
NAME                 PROVISIONER            AGE
standard (default)   kubernetes.io/cinder   268d
```

```
kubectl get storageclasses.storage.k8s.io
NAME                   PROVISIONER                AGE
cinder-csi (default)   cinder.csi.openstack.org   6h45m
```

The provisioner is version and creation time dependent.

* `kubernetes.io/cinder`
    all Kubernetes Cluster prior 1.16 and created before 29.10

* `cinder.csi.openstack.org`
    all Kubernetes Cluster 1.16+ and created after 19.10

## Openstack Volume Types

The Openstack volume types sorted by maximum possible IOPS:

* low-iops
* default <- used in the default class
* high-iops

### Add your own classes

If you need use one of the other types, you can add your own definitions.

Example:
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: my-high-iops-class
provisioner: cinder.csi.openstack.org
parameters:
  type: high-iops
```
and apply with `kubectl apply -f storage-class.yaml`

* `name` you should choose a unique one, as we don't want to interfere with the default names.
* `provisioner` use the one of your cluster. You can always have a look in the default class to verify the right provider.
* `type` use on of the official provides types from the optimist platform (as of writing low-iops and high-iops).

To use the new storage class you need to change your volumes definitions and add the new StorageClass name.
