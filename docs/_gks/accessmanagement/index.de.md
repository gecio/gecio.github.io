---
title: Access Management
lang: de
permalink: /gks/accessmanagement/
nav_order: 6000
has_children: true
---
<!-- LTeX:  language=de-DE -->
# Access Management

Prinzipiell gibt es zwei Möglichkeiten, Benutzern Zugriff auf einen GKS Cluster zu gewähren:

* Zugriff auf ein GKS-Projekt (und damit aller enthaltenen Cluster) geben.
* Über "Role-Based Access Control" (RBAC) direkten Zugriff auf den Cluster bzw. einzelne Namespaces geben

## Zugriff via GKS-Projekt

> **Hinweis:** Dies ist die empfohlene Methode, anderen Benutzern Zugriff zu gewähren.

Erteilt man Benutzern Zugriff auf ein gesamtes GKS-Projekt (wie [hier](/gks/managingprojects/creatingaproject/) beschrieben), bekommen Benutzer automatisch Zugriff auf alle Cluster innerhalb dieses Projektes. Die Benutzer können sich in GKS einloggen und nach Auswahl des Projektes alle Cluster sehen bzw. je nach gewährten Zugriffsrechten auch bestehende Cluster editieren oder neue Cluster anlegen. Weiterhin können die Benutzer auch ihre `kubeconfig`-Datei direkt herunterladen:

![Download kubeconfig](download_kubeconfig.png)

Dabei teilen sich alle Benutzer mit den gleichen Zugriffsrechten effektiv eine `kubeconfig`-Datei, da in diesem Fall eine Token-basierte Authentifizierung genutzt wird und je Access-Level ein Token erstellt wird. Wenn der Zugriff für einen Benutzer mit projektweitem Zugriff entzogen werden soll, muss daher die `Revoke Token`-Funktion des Clusters genutzt werden. Bestehende Token werden dann ungültig - und in der Folge müssen sich alle verbliebenen Benutzer eine neue `kubeconfig`-Datei herunterladen.

## Role-based Access Control (RBAC)

Die RBAC-Funktion erlaubt es einem GKS-Projekt-Admin, einen feingranulareren Zugriff basierend auf vordefinierten `ClusterRoles` und `Roles` zu gewähren. Über das GKS Dashboard kann ein Admin dafür clusterweit gültige `ClusterRoleBindings` und nur in einem bestimmten Namespace gültige `RoleBindings` anlegen:

![RBAC option](rbac.png)

Benutzer mit diesem Zugriffslevel wiederum haben keinen Zugriff auf das GKS Dashboard, können sich jedoch über einen bestimmten Link (s.u.) ihre eigenen kubeconfig-Datei herunterladen und damit Zugriff auf das Cluster bekommen.

Mehr Informationen über Kubernetes RBAC finden Sie [hier](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).

## Login Tokens resetten

Zum Rotieren des Login-Tokens.

![Revoke Menu](revoke-token-menu.png)

Hier den Token auswählen und den revoke button klicken.

![Revoke Popup](revoke.png)

Anschließend eine neue [Kubeconfig][#Zugriff via GKS-Projekt] herunterladen, die alte ist jetzt ungültig.

## Weiterführende Themen

* [Projektzugriff: Mit einem Cluster verbinden](/gks/accessmanagement/connectingtoacluster/)
* [Role-Based Access Control (RBAC)](/gks/accessmanagement/usingrbac/)
