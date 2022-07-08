---
title: Ein PVC von einem existierenden Openstack Volume wiederherstellen
lang: de
permalink: /gks/clusterbackups/restorepvcfromvolume/
nav_order: 5200
parent: Cluster-Backups
---
<!-- LTeX:  language=de-DE -->

# Ein PVC von einem existierenden Openstack Volume wiederherstellen

Wird ein PVC (PersistentVolumeClaim) in einem unserer Cluster angelegt, wird daraufhin normalerweise automatisch ein *neues* PV (PersistentVolume) in Kubernetes sowie ein entsprechendes *neues* Volume in Openstack angelegt. Das neue Volume ist leer und kann direkt genutzt werden. Es ist allerdings auch möglich, ein *bereits existierendes* Openstack-Volume in einem Kubernetes-Cluster zu benutzen. Dieses Dokument beschreibt eine mögliche Vorgehensweise wie dies erreicht werden kann.

## Voraussetzungen

Als wichtigste Voraussetzung sollte natürlich ein freies, aktuell nicht benutztes Openstack-Volume vorhanden sein. Dies könnte zum Beispiel der Fall sein, wenn ein altes Cluster gelöscht, aber im Löschen-Dialog die Option explizit selektiert wurde dass verbundene Volumes *nicht* gelöscht werden oder wenn beispielsweise ein Volume von einem Cluster in ein anderes umgezogen werden soll. In jedem Fall wird es nicht um das Volume an sich, sondern um die darauf vorhandenen Daten gehen, die dem Kubernetes-Cluster jetzt als PVC zur Verfügung gestellt werden sollen.

Um ein existierendes Openstack-Volume als PVC in ein Kubernetes-Cluster einzubinden, benötigen wir die ID des Volumes. Um diese herauszufinden, müssen wir uns zuerst in das [Openstack/Optimist Dashboard](https://dashboard.optimist.innovo.cloud/auth/login/) einloggen:

![Openstack Login](openstack-1.png)

Die Zugangsdaten sind identisch zu den Zugangsdaten, die beim GKS/GKS-Dashboard benutzt werden. Nach dem Login navigieren wir zu `Volumes` und suchen das Volume, welches wir anbinden wollen. Das Volume sollte aktuell an keine andere Instanz/VM gebunden sein und den Status "Available" haben. (Da ein Volume immer nur an eine Instanz gebunden sein kann, sollte das Volume ggf. erst von der alten Instanz getrennt/detached werden, bevor es im Kubernetes-Cluster verwendet werden kann.)

![Openstack Volume](openstack-2.png)

Nachdem wir das Volume gefunden haben, klicken wir auf den Namen. Auf der Detailseite findet sich dann die ID des Volumes, die wir für den nächsten Schritt benötigen:

![Openstack Volume ID](openstack-3.png)

## Erstellen eines PV auf Basis eines existierenden Openstack-Volumes

Um das Volume im Cluster verfügbar zu machen muss im ersten Schritt ein `PersistentVolume`-Manifest vorbereitet werden, welches die ID des Openstack-Volumes im `spec.csi.volumeHandle`-Schlüssel enthält:

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: test-pv-restore
spec:
  accessModes:
  - ReadWriteOnce
  capacity:
    storage: 3Gi
  csi:
    driver: cinder.csi.openstack.org
    volumeHandle: 6515d33b-287d-43e1-a3c5-e347d2fc8135
  persistentVolumeReclaimPolicy: Delete
  storageClassName: cinder-csi
  volumeMode: Filesystem
```

Anschließend erstellen wir das PV via `kubectl` und überprüfen, ob das PV erfolgreich erstellt wurde:

```bash
# kubectl apply -f restore-pv.yaml
persistentvolume/test-pv-restore created
# kubectl get pv
NAME              CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS      CLAIM   STORAGECLASS   REASON   AGE
test-pv-restore   3Gi        RWO            Delete           Available           cinder-csi              3s
```

In diesem Beispiel haben wir das PV `test-pv-restore` erstellt, das ein bereits existierendes Openstack-Volume referenziert.

## Erstellen eines PVC, das auf das korrekte PV verweist

Im nächsten Schritt müssen wir ein PVC erstellen, welches das eben erzeugte PV referenziert. Um dies zu erreichen, müssen wir im PVC-Manifest den Schlüssel `spec.volumeName` auf den Namen des gerade erstellten PVs setzen:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: test-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 3Gi
  volumeName: test-pv-restore
```

Nach dem Erstellen des PVCs via `kubectl` sollte sich das PVC im Status "Bound" befinden:

```bash
# kubectl apply -f restore-pvc.yaml
persistentvolumeclaim/test-pvc created
# kubectl get pvc
NAME       STATUS   VOLUME            CAPACITY   ACCESS MODES   STORAGECLASS   AGE
test-pvc   Bound    test-pv-restore   3Gi        RWO            cinder-csi     2s
```

Das PVC "test-pvc" ist damit zur Verwendung bereit.

## Erstellen eines Test-Pods, der das PVC benutzt

Um die Daten vor der tatsächlichen Verwendung zu überprüfen, können wir einen Test-Pod erstellen, der das PVC benutzt:

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pod
spec:
  containers:
    - name: test-pod
      image: ubuntu
      command:
        - "sleep"
        - "604800"
      volumeMounts:
        - mountPath: "/restore"
          name: test-pvc
  volumes:
    - name: test-pvc
      persistentVolumeClaim:
        claimName: test-pvc
```

Der wichtige Teil des Manifests ist dabei, dass der `claimName` korrekt auf den Namen des eben erstellten PVCs gesetzt ist. Nachdem der Pod via `kubectl` erzeugt wurde, können wir uns mit dem Pod verbinden. Das Volume ist im o.g. Beispiel im Pfad */restore* gemounted, so dass wir die Daten des Volumes dort finden:

```bash
# kubectl apply -f pvc-example/test-pod.yaml
pod/test-pod created
# kubectl get pod -w
NAME       READY   STATUS              RESTARTS   AGE
test-pod   0/1     ContainerCreating   0          5s
test-pod   0/1     ContainerCreating   0          17s
test-pod   1/1     Running             0          22s
^C
# kubectl exec -ti test-pod -- /bin/bash
root@test-pod:/# ls /restore/
lost+found  my_data.txt
root@test-pod:/# exit
exit
```

Damit haben wir erfolgreich ein bereits existierendes Openstack-Volume an einen Pod in unserem Kubernetes-Cluster angebunden.
