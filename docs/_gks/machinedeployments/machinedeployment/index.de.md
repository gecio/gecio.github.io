---
title: Machine Deployments
lang: de
permalink: /gks/machinedeployments/machinedeployment/
nav_order: 6100
parent: Machine Deployments
---
<!-- LTeX:  language=de-DE -->
# Machine Deployments

## Machine Deployment hinzufügen

Um ein Machine Deployment hinzuzufügen, klicken Sie auf die `Add Machine Deployment` Schaltfläche in der oberen rechten Ecke.

![add_machine_deployment](../images/MachDepl01.png)

Der `Add Machine Deployment` Dialog erscheint.
Fügen Sie Ihre Daten hinzu und klicken Sie auf `Add Machine Deployment`.

![add_dialog](../images/MachDepl03.png)

Die neuen Nodes werden jetzt angelegt. Den aktuellen Status bekommen Sie in den Machine Deployment Details.

Wählen Sie das neue Machine Deployment aus.

![machine_deployment_overview](../images/MachDepl04.png)

Warten Sie bis alle Nodes grün sind.

![machine_deployment_status](../images/MachDepl05.png)

## Machine Deployments löschen

Um ein Machine Deployment zu löschen, können Sie das Löschsymbol in der Machine Deployment Liste benutzen.

![delete_from_list](../images/MachDepl06.png)

Sie können auch das Löschsymbol auf der Detailseite verwenden.

![delete_from_details](../images/MachDepl07.png)

## Machine Deployments umbenennen

Machine Deployments können nicht umbenannt werden.

Es muss daher ein neues Machine Deployment [angelegt](#machine-deployment-hinzufügen) werden und anschließend das alte [gelöscht](#machine-deployments-löschen) werden.

Dabei kann es aber, je nach Replica Einstellung und Anzahl der Nodes, zu kurzen Ausfällen kommen.

Sicherer ist es, wenn das alte Machine Deployment langsam reduziert wird - 1 Replica nach dem anderen – bis keine Node mehr übrig ist und dann erst löscht. Bei der Prozedur kann es passieren, dass Pods nicht nur auf die neuen Nodes umgezogen werden, sondern auch auf noch aktive alte. Dann wird ein Pod ggf. mehrfach umgezogen. Das können Sie umgehen, indem Sie vor dem Reduzieren erst alle alten Nodes mit `kubectl cordon <node name>` aus dem Scheduler entfernen.
