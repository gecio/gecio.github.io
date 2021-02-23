---
title: StorageClass Setup
lang: de
permalink: /imke/nodedeployments/storageclasses/
nav_order: 5500
parent: Node Deployments
---

Es gibt eine Default Storage Classe pro Cluster

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

Der Provisioner is abhängig von der erstellung des Clusters and der Kubernetes Version

* kubernetes.io/cinder  
    Alle Kubernetes Cluster kleiner 1.16 und vor dem 29.10 angelegt.    
    
* cinder.csi.openstack.org  
    Alle Kubernetes Cluster 1.16+ und nach dem 19.10 angelegt.
    
Beide sind default StorageClasses für Volumes.

## Openstack Volume Types

* default <- wird in der Default Class verwendet
* low-iops
* high-iops

We der Name schon andeutet, eine hat mehr IOPS QoS, eine weniger :-)

### Eine eige Klasse anlegen

Wenn eine der beiden ander Typen benötigt wird, kann man sich eigene Definitionen anlegen.

bsp.
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: my-high-iops-class
provisioner: cinder.csi.openstack.org
parameters:
  type: high-iops
```
und anlegen mit `kubectl apply -f storage-class.yaml`

* `name` hier muss ein eigener Name verwendend werden, damit es nicht mit den Standardklassen kollidiert.  
* `provisioner` Benutzt immer den aktuellen Provisioner. Man findet ihn in der Standardklasse.  
* `type` Benutze immer eine offiziell vom Optimist unterstützten Disk-Type (aktuell low-iops and high-iops). 

Um die neue Klasse zu verwenden, muss man die Volume definition mit dem neuen Namen anpassen.
