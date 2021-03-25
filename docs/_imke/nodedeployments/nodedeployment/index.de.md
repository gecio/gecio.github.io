---
title: Node Deployments
lang: de
permalink: /imke/nodedeployments/nodedeployment/
nav_order: 5100
parent: Node Deployments
---

# Node Deployment hinzufügen

Um ein Node Deployment hinzuzufügen `Add Node Deployment Button` in der oberen rechten Ecke klicken.

![add_node_deployment](add_nodedep.png)

Das öffnet den  `Add Node Deployment` Dialog.

![add_dialog](add_dialog.png)

Zum Speichern `Add Node Deployment` klicken.

![add_button](add_button.png)

The new nodes will be created. You can look at the progress in the node deployment details.
Die neuen Nodes werden jetzt angelegt. Den aktuellen Status bekommen man in der Node Deployment Details.  

Das neue Node Deployment auswählen

![node_deployment_overview](node_deployment_overview.png)

und warten bis alle nodes Grün sind.

![node_deployment_status](node_deployment_status.png)


# Node Deployments löschen

Um ein Node Deployment zu löschen, kann man das Löschsymbol in der Node Deployment Liste nehmen

![delete_from_list](delete_from_list.png)

oder auf der Detailseite.

![delete_from_details](delete_from_details.png)


# Node Deployments umbenennen

Node deployment können nicht umbenannt werden. 

Es sollte ein Neues [angelegt](#node-deployment-hinzufgen) werden und anschließend das Alte [gelöscht](#node-deployments-lschen) werden.

Dabei kann es aber, je nach replica setting und anzahl der Nodes, zu ausfällen kommen.

Sicherer ist es, wenn man das alte Node Deployment langsam reduzieren - 1 replica nach dem anderen – bis keine Node mehr übrig ist und dann erst löschen. 
Bei der Prozedur kann es passieren das Pods nicht nur auf die neuen Nodes umgezogen werden, sondern auch auf noch aktive alten. Dann wird ein Pod ggf mehrfach umgezogen.
Das kann man um gehen in dem man vor dem Reduzieren erst alle alten Nodes mit, `kubectl cordon <node name>` aus dem Scheduler entfernt. 
