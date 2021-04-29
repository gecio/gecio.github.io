---
title: SSH Key einem Cluster hinzufügen
lang: de
permalink: /imke/nodedeployments/add_ssh_key/
nav_order: 5400
parent: Machine Deployments
---

Um sich auf einer Worker Node anmelden zu können muss man einen SSH-Key hinzufügen.

Dafür benötigt man folgende Schritte:

- Einen SSH-Key erstellen
- Ihn dem Projekt hinzufügen
- Dem Cluster hinzufügen

## Einen SSH-Key erstellen

Ein Schlüsselpaar kann man am einfachsten mit dem Tool `ssh-keygen` erzeugen:

```bash
ssh-keygen
```

Die erzeugten Schlüssel (öffentlich und privat) werden standardmäßig in `.ssh/id_rsa.pub` abgelegt.

## Den SSH Key dem Projekt hinzufügen

1. Zuerst muss das richtige Porjekt ausgewählt werden:

    ![Projects](projects.png)

2. Danach zur SSH-Key-Seite gehen:

    ![Project-Menu](project-menu.png)

3. Dort benutzen Sie bitte die "Add SSH Key"-Schaltfläche:

    ![SSH-Key-Page](ssh-key-page.png)

4. Zur leichteren Identifikation geben Sie dem Key bitte einen Namen und fügen Sie den öffentlichen (nicht den privaten!)
   Schlüssel ind as dafür vorgesehene Feld ein:

    ![Ssh-key](ssh-key.png)

Jetzt kann man den Key in jeden Cluster des Projekts benutzen.
Dies gilt auch für die Erstellung eines neuen Clusters im gleichen Projekt.

## Den SSH Key einem Cluster hinzufügen

1. Cluster auswählen:

    ![Cluster](clusters.png)

2. Umdas Cluster-Menü zu öffnen, bitte auf die drei Punkte klicken:

    ![Trhee-Dots](three-dots.png)

3. Aus dem Menü bitte `Manage SSH keys` wählen:

    ![Edit-Cluster](manage-ssh-keys.png)

4. Nach einem Klick auf `+ Add SSH key` kann der eben erstellte SSH-Key aus einer Drop-Down Liste ausgewählt werden.

    ![Manage-Keys](manage-keys.png)

5. Um den Key nun hinzuzufügen, muss abschließend noch der `Add SSH Key`-Button gedrückt werden:

    ![Key-List](key-list.png)

Dein Key wird nur allen Worker-Nodes in allen Machinedeployments hinzugefügt.

## Am Worker anmelden

Um sich per SSH anmelden zu können, brauchen die Worker eine öffentliche (Floating) IP.

Dazu Editieren Sie die Machinedeployments:

![Edit-MD](edit_machine_deployment.png)

Dort sollten Sie sicher stellen, dass `Allocate Floating IP` aktiviert ist:

![Enable-Floating_IP](enable-fip.png)

Wenn sich hier ein Setting ändert, werden alle Worker neu erstellt. Danach kann man sich per SSH einloggen.

Der Standarduser für Ubuntu ist dabei `ubuntu` und für Flatcar `core`.

```bash
 ssh -A ubuntu@PUBLIC_IP
 ssh -A core@PUBLIC_IP
```
