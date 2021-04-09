---
title: Resizing Volumes
lang: en
permalink: /imke/k8sapplications/resizevolumes/
nav_order: 7600
parent: Kubernetes Applications
---

Resizing existing and even in-use volumes is supported by iMKE.

### Update StorageClass

To allow the resizing of volumes, the `StorageClass` which was used to create the volume must include `allowVolumeExpansion: true`.

To patch the default `cinder-csi`-storageclass with this field, you can run the following command:

```bash
kubectl patch storageclass cinder-csi --type=json -p='[{"op": "add", "path": "/allowVolumeExpansion", "value": true}]'
```

This should produce the following message:

```bash
storageclass.storage.k8s.io/cinder-csi patched
```

### Resizing the PVC

After this is done, you can edit/update the PVC even if it still in use by a pod and set `spec.resources.requests.storage` to the increased value (i.e. using `kubectl edit pvc`):

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

After you updated `storage` to a bigger size, the underlying volume should get immediately get increased by the storage provider. To complete the process, the file-system also needs to be expanded. This will happen automatically when a Pods starts to use the PVC, which means if you change an in-use volume that will happen immediately as well.
