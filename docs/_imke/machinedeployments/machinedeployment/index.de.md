---
title: Machine Deployments
lang: de
permalink: /imke/machinedeployments/machinedeployment/
nav_order: 5100
parent: Machine Deployments
---
<!-- LTeX:  language=de-DE -->

# Machine Deployment hinzufügen

Um ein Machine Deployment hinzuzufügen, `Add Machine Deployment Button` in der oberen rechten Ecke klicken.

![add_node_deployment](add_nodedep.png)

Das öffnet den  `Add Machine Deployment` Dialog:

![add_dialog](add_dialog.png)

Zum Speichern `Add Machine Deployment` klicken:

![add_button](add_button.png)

Die neuen Nodes werden jetzt angelegt. Den aktuellen Status bekommen man in der Machine Deployment Details.  

Das neue Machine Deployment auswählen:

![node_deployment_overview](node_deployment_overview.png)

…und warten bis alle nodes Grün sind:

![node_deployment_status](node_deployment_status.png)

# Machine Deployments löschen

Um ein Machine Deployment zu löschen, kann man das Löschsymbol in der Machine Deployment Liste nehmen:

![delete_from_list](delete_from_list.png)

…oder auf der Detailseite:

![delete_from_details](delete_from_details.png)

# Machine Deployments umbenennen

Machine Deployments können nicht umbenannt werden.

Es sollte ein neues Machine Deployment [angelegt](#node-deployment-hinzufgen) werden und anschließend das alte [gelöscht](#node-deployments-lschen) werden.

Dabei kann es aber, je nach replica setting und anzahl der Nodes, zu Ausfällen kommen.

Sicherer ist es, wenn man das alte Machine Deployment langsam reduziert - 1 replica nach dem anderen – bis keine Node mehr übrig ist und dann erst löscht. Bei der Prozedur kann es passieren das Pods nicht nur auf die neuen Nodes umgezogen werden, sondern auch auf noch aktive alten. Dann wird ein Pod ggf mehrfach umgezogen. Das kann man umgehen, indem man vor dem Reduzieren erst alle alten Nodes mit `kubectl cordon <node name>` aus dem Scheduler entfernt.
