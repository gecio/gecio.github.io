---
title: Revoking Tokens
lang: "de"
permalink: /gks/accessmanagement/revokingtoken/
nav_order: 7300
parent: Access Management
---
# Revoking Tokens

Alle Benutzer mit den gleichen Zugriffsrechten auf ein gesamtes [GKS-Projekt](/gks/managingprojects/creatingaproject/) teilen sich die gleiche `kubeconfig`-Datei. Die `kubeconfig`-Datei, nutzt eine Token-basierte Authentifizierung, und abhängig vom Access-Level wird ein Token erstellt. Wenn der Zugriff einem Benutzer mit projektweitem Zugriff entzogen werden soll, muss daher die `Revoke Token`-Funktion des Clusters genutzt werden. Bestehende Token werden dann ungültig und alle verbliebenen Benutzer müssen sich eine neue`kubeconfig`-Datei herunterladen.

## Login Token Resetten

Zum Rotieren des Login-Tokens führen Sie folgende Schritte aus:

1. Wählen Sie Ihren Cluster aus.

    ![Step 1](../images/ConnClus01.png)

2. Klicken Sie auf der Cluster-Detailseite auf die drei Punkte, um das Cluster-Untermenü zu öffnen. Wählen Sie anschließend `Revoke Token`.

    ![Revoke Token](../images/RevTok01.png)

3. Wählen Sie den Token aus und klicken Sie auf `Revoke Token`.

    ![Revoke Token](../images/RevTok02.png)

4. Laden Sie anschließend eine neue `kubeconfig`-Datei herunter, da die vorherige nun ungültig ist.
