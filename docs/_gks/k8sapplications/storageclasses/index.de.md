---
title: StorageClass Setup
lang: de
permalink: /gks/k8sapplications/storageclasses/
nav_order: 8500
parent: Anwendungen in Kubernetes
---
<!-- LTeX:  language=de-DE -->
# StorageClass Setup

Es gibt eine vorinstalliere Default Storage Class pro Cluster.
> **Achtung:**
> Diese wird von GKS verwaltet und kann **jederzeit überschrieben** werden. Bitte erstellen Sie für Änderungen eine eigene Storage Class.

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

Der Provisioner ist abhängig vom Erstellungszeitpunkt des Clusters und der Kubernetes Version.

* `kubernetes.io/cinder`
    alle Kubernetes Cluster kleiner 1.16 und vor dem 29.10 angelegt.

* `cinder.csi.openstack.org`
    alle Kubernetes Cluster 1.16+ und nach dem 19.10 angelegt.

## Openstack Volume Types

Die Openstack Volume Types nach maximal möglichen IOPS sortiert:

* low-iops
* default <- wird in der Default Class verwendet
* high-iops

### Eine eigene Klasse anlegen

Wenn eine der beiden anderen Typen benötigt wird, können Sie sich eigene Definitionen anlegen.

Beispiel:

```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: my-high-iops-class
provisioner: cinder.csi.openstack.org
parameters:
  type: high-iops
```

Zum Anlegen verwenden Sie das Kommando: `kubectl apply -f storage-class.yaml`

* `name`: Hier muss ein eigener Name verwendet werden, damit es nicht mit den Standardklassen kollidiert.
* `provisioner`: Benutzt immer den aktuellen Provisioner. Man findet ihn in der Standardklasse.
* `type`: Benutzt immer eine offiziell vom Optimist unterstützten Disk-Type (aktuell low-iops and high-iops).

Um die neue Klasse zu verwenden, passen Sie die Volume Definition mit dem neuen Namen an.
