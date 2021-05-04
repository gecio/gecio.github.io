---
title: Volumes vergrößern
lang: de
permalink: /imke/k8sapplications/resizevolumes/
nav_order: 7600
parent: Anwendungen in Kubernetes
---

Das Vergößern wird durch die iMKE-Plattform auch für aktuell genutzte Volumes unterstützt.

### Aktualisierung der StorageClass

Um das Vergrößern von Volumes zu aktivieren, muss in der `StorageClass` mit der das Volume erstellt wurde, die Option `allowVolumeExpansion: true` gesetzt sein.

Um die voreingestellte `cinder-csi`-StorageClass um dieses Feld zu ergänzen, kann der folgende Befehl ausgeführt werden:

```bash
kubectl patch storageclass cinder-csi --type=json -p='[{"op": "add", "path": "/allowVolumeExpansion", "value": true}]'
```

Dies sollte die folgende Meldung produzieren:

```bash
storageclass.storage.k8s.io/cinder-csi patched
```

### Vergrößern des PVC

Nach dem Anpassen der o.g. Einstellung kann das zu betreffende PersistentVolumeClaim (PVC) direkt editiert werden – auch wenn es noch durch einen Pod benutzt wird. Um die Vergrößerung vorzunehmen, muss lediglich der Wert in `spec.resources.requests.storage` angepasst werden (bspw. mittels `kubectl edit pvc`):

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
...
spec:
  ...
  resources:
    requests:
      storage: 20Gi
  storageClassName: cinder-csi
```

Nachdem `storage` auf einen größeren Wert angepasst wurde, wird der zugrundeliegende Storage Provider automatisch das Volume vergrößern. Um den Prozess abzuschließen, muss allerdings auch noch das Dateisystem erweitert werden, was automatisch passiert, sobald das PVC von einem Pod benutzt wird. PVCs die während der Änderung an einen Pod gebunden sind, werden sofort und ohne Pod-Neustart aktualisiert.
