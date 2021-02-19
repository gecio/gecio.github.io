---
title: StorageClass Setup
lang: en
permalink: /imke/nodedeployments/storageclasses/
nav_order: 4
parent: Node Deployments
---

We are providing one default storage class per Cluster.

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

* kubernetes.io/cinder  
    all Kubernetes Cluster prior 1.16 and created before 29.10
    
* cinder.csi.openstack.org
    All Kubernetes Cluster 1.16+ and created after 19.10
    
Both are the default StorageClasses for Volumes.

## Openstack Volume Types

* default <- used in the default class
* low-iops
* high-iops

As the naming suggest one has lower and one better IOPS QoS :-)

### Add your own classes

If you need use one of the other types you can add your own definitions.

f.e
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

To use the new class you need to change pur volumes definitions and add the new StorageClass Name
