---
title: Einführung
lang: "de"
permalink: /ece/intro
has_children: false
nav_order: 1000
---

# Einführung

Elastic Cloud Enterprise by German Edge Cloud (ECE) ist eine von GEC gehostete und betriebene Cloud-Plattform, auf der Sie Ihre Elastic-Workloads in Form sogenannter Deployments betreiben können.

Ein Deployment ist ein isoliertes Elastic Cluster, das mit ECE verwaltet wird. Es besteht aus mehreren Komponenten des Elastic Stacks. Für jedes Deployment können Sie folgendes individuell festlegen:

- Welche der unten aufgeführten Elastic Stack Komponenten verwendet werden sollen
- Die Größe der jeweiligen Komponenten
- Die Anzahl der Availability Zones (1 bis 3) über die sie verteilt werden sollen
- Welche der von GEC bereitgestellten Versionen für den Elastic Stack verwendet werden soll

Jedes Deployment bekommt eigene, aus dem Internet erreichbare URLs für Elasticsearch, Kibana, Fleet und Enterprise Search.

## Komponenten des Elastic Stacks

ECE enthält alle aktuell von Elastic angebotenen Server-Komponenten:

- Elasticsearch in den 4 Tiers Hot, Warm, Cold und Frozen sowie Master Nodes und Coordinating/Ingest Nodes
- Kibana
- Machine Learning
- Integration Server (Fleet und APM)
- Enterprise Search

Die Definition der Elasticsearch Data Tier finden Sie hier: [https://www.elastic.co/guide/en/elasticsearch/reference/current/data-tiers.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/data-tiers.html).

## Funktionsumfang der Komponenten

Alle ECE Deployments sind mit der Elastic Enterprise Lizenz ausgestattet, d.h. alle Komponenten enthalten alle Funktionalitäten der höchstwertigen Elastic-Lizenz. Dazu gehören Machine Learning, durchsuchbare Snapshots im Frozen Tier (eine sehr kostengünstige Variante für die Speicherung selten abgerufener Daten), Cross-Cluster-Search, alle SIEM (Security Information and Event Management) Funktionen des Elastic Stacks, Watcher und Alerting, der neue AI Assistant u.v.m.

Einen Überblick über die Komponenten finden Sie hier [https://www.elastic.co/de/pricing/](https://www.elastic.co/de/pricing/). Eine detaillierte Auflistung erhalten Sie hier: [https://www.elastic.co/de/subscriptions](https://www.elastic.co/de/subscriptions).

## Berechnung der Größe des Deployments und Preismodell

Die Größe des Deployments wird in GB RAM gemessen. Jeder Elastic Komponente wird nach einem bestimmten Schlüssel zum RAM entsprechend CPU und Storage zugeordnet. Dieser Zuordnungsschlüssel kann pro Komponente verschieden sein. Für die meisten Anwendungsfälle ist der Arbeitsspeicher der bestimmende Faktor für die Performance eines Elastic Workloads. Da die Zuordnung nach festen Schlüsseln erfolgt, reicht ein Parameter (GB RAM) aus, um die anderen Parameter (CPU, Storage) zu berechnen. Dadurch wird auch das Preismodell sehr transparent und leicht verständlich. Sie müssen lediglich den Arbeitsspeicher des Deployments kennen, um Ihre Kosten zu berechnen.

Sie zahlen für die genutzten GB RAM Ihres Deployments. Die Erfassung der Nutzung und die Abrechnung erfolgen stundengenau.

Zusätzlich fallen Kosten für den genutzten Snapshot-Storage in unserem Object Storage Cluster an. Der (ein- und ausgehende) Datenverkehr ist kostenfrei.

## Autoscaling Feature

Die Komponenten Elasticsearch und Machine Learning verfügen über ein sogenanntes Autoscaling. Dieses Feature haben Sie bei einem selbst gehosteten Elastic Stack in dieser Form nicht oder müssten es selbst entwickeln. Mit Autoscaling überwacht ECE den genutzten Festplattenspeicher der Elasticsearch-Komponenten und fügt bei Überschreiten eines Schwellenwertes automatisch weitere Ressourcen hinzu. Dadurch müssen Sie auch bei Wachstum Ihrer Datenmenge keine Ausfälle wegen voller Festplatten o.ä. befürchten. Machine Learning Workloads werden in der notwendigen Größe nur dann gestartet, wenn sie auch benötigt werden. Die Ressourcen in Ihrem Deployment und somit auch Ihre Kosten wachsen mit Ihrer Datenmenge. Dadurch wird eine effiziente Ausnutzung der Ressourcen und eine kosteneffiziente Abbildung Ihrer Workloads auf der ECE Plattform möglich.

Für jede Komponente mit Autoscaling lassen sich auch Obergrenzen definieren bis zu denen automatisch hochskaliert wird. Somit gerät z.B. bei Fehlkonfigurationen Ihr Rechnungsbetrag nicht außer Kontrolle.

## Ihre Vorteile durch ECE von GEC

Mit der ECE-Lösung von GEC erhalten Sie alle Funktionalitäten der Software von Elastic, dem Marktführer bei Suche, SIEM und Observability, inklusive Enterprise-Lizenz. GEC hostet ausschließlich in hochsicheren deutschen Rechenzentren und leistet alle Betriebsaufgaben von Deutschland aus. Sie erhalten alles aus einer Hand von einem deutschen Vertragspartner. Im Gegensatz zu einem "klassischen" Deployment des Elastic Stacks in einem Cluster mit virtuellen Maschinen, die von der jeweiligen Cloud-Plattform auf vorgegebene Größen beschränkt sind, werden bei ECE Docker-Container verwendet. Diese können bei Bedarf automatisch skalieren. Durch das feingranulare Autoscaling können Sie die Ressourcen effizient ausnutzen und die Workloads kosteneffizient betreiben.

## Leistungen der GEC

- GEC stellt in seinen sicheren und zertifizierten Rechenzentren in Deutschland die Cloud-Umgebung bereit, auf denen Ihre Elastic Workloads laufen.
- GEC gibt Ihnen die Möglichkeit, zwecks hoher Verfügbarkeit Ihre Workloads über 3 Verfügbarkeitszonen (Availability Zones) zu verteilen.
- GEC kümmert sich darum, dass genügend Ressourcen für die Skalierung Ihres Deployments zur Verfügung stehen.
- GEC überwacht die ECE-Umgebung und sorgt dafür, dass sie verfügbar ist (99,85 %). Auch außerhalb der Bürozeiten wird automatisch eine Rufbereitschaft alarmiert, um eventuelle Probleme an der ECE-Plattform möglichst schnell zu beheben.
- GEC sorgt für regelmäßige Updates und Security-Patches der Server.
- GEC stellt neue Versionen des Elastic Stacks zur Installation bereit, meist nur wenige Werktage nachdem diese von Elastic veröffentlicht wurden. Wir führen jedoch keine automatischen oder unaufgeforderten Versions-Upgrades Ihres Deployments durch (ausgenommen EOL-Versionen).
- GEC stellt aus dem Internet erreichbare URLs für alle Deployments inklusive TLS-Zertifikat für die verschlüsselte Kommunikation zur Verfügung (kundeneigene Zertifikate sind leider nicht möglich).
- GEC stellt Backup- und Snapshot-Storage im redundanten GEC Object Storage Cluster zur Verfügung. Für jedes Deployment wird standardmäßig eine (von Ihnen änderbare) Snapshot Policy aktiviert, über die automatisch in regelmäßigen Abständen eine Datensicherung Ihrer Elasticsearch-Daten in den Object Storage erfolgt. Dadurch sind Sie sehr gut gegen Datenverlust geschützt.
- All Ihre Deployments in der ECE-Cloud von GEC sind mit der Elastic Enterprise Lizenz ausgestattet. Sie müssen keine Lizenzen bei Elastic einkaufen, Sie erhalten von GEC alles aus einer Hand.

## Ihre Verantwortlichkeit als Kunde

Als Kunde sind Sie für alles verantwortlich was **innerhalb** Ihres Deployments passiert, insbesondere:

- Das Einsammeln von Daten aus Client-Systemen und das Laden dieser Daten in Elasticsearch
- Die Konfiguration von Lifecycle Policies
- Das Anlegen von Nutzern und die Vergabe von Berechtigungen
- Die Überwachung der Shard-Gesundheit und bei Bedarf deren Reparatur (s. hierzu die Dokumentation von [elastic.co](https://www.elastic.co/))
- Konfiguration von Alerts - sofern gewünscht
- Wiederherstellung von Snapshots bei Bedarf
- Überwachung der Performance Ihres Deployments, sofern Sie dafür spezielle Anforderungen haben. Welche Performance für Ihren Anwendungsfall akzeptabel ist, können wir nicht wissen.

Weitere Bestimmungen entnehmen Sie den Allgemeinen Geschäftsbedingungen der GEC.

## Service Description

Für detailliertere Informationen kontaktieren Sie bitte unseren Vertrieb.

## Auszuführende Schritte nach dem ECE-Deployment

Wir empfehlen Ihnen, folgende Schritte durchzuführen, um die Sicherheit Ihres ECE Deployments zu erhöhen und Daten in Elastic zu laden:

### Erhöhung der Sicherheit

- Legen Sie in Kibana die benötigten Spaces, Rollen und Nutzer an.
Weitere Informationen dazu finden Sie hier: [https://www.elastic.co/guide/en/kibana/current/tutorial-secure-access-to-kibana.html](https://www.elastic.co/guide/en/kibana/current/tutorial-secure-access-to-kibana.html).
- Verwenden Sie nicht den elastic Superuser für die tägliche Arbeit in Kibana oder um Daten anzuliefern.
- Sofern gewünscht, konfigurieren Sie Single Sign-On mittels [SAML](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-securing-clusters-SAML.html), [LDAP](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-securing-clusters-ldap.html), [Active Directory](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-securing-clusters-ad.html), [OpenID Connect](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-secure-clusters-oidc.html) oder [Kerberos](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-secure-clusters-kerberos.html) sowie dynamische [role mappings](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-roles.html), wenn Sie Role-based oder Attribute-based Access Control benötigen. Für die Einstellung der Parameter in Ihrem Deployment wenden Sie sich vorläufig an unseren Support.
- Falls gewünscht, lassen Sie durch unseren Support [traffic filter](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-traffic-filtering-deployment-configuration.html) konfigurieren. Damit können Sie unerwünschte Zugriffe auf Ihr Deployment verhindern.
- Legen Sie ggf. (falls Sie nicht den Agent verwenden, s.u.) Service Accounts für die Datenanlieferung an und erzeugen Sie dafür API Keys. Weitere Informationen finden Sie hier: [Grant access using API keys](https://www.elastic.co/guide/en/beats/filebeat/current/beats-api-keys.html).

### Laden von Daten in Elastic

- Laden Sie Daten in Ihren Cluster.
Wir empfehlen hierfür den Elastic Agent in Zusammenarbeit mit dem Fleet Server. Eine detaillierte Anleitung finden Sie hier: [Laden von Daten in ECE](/ece/shipdata).
- Um Verbindungsprobleme zu ihrem Cluster zu vermeiden, stellen sie sicher, [Sniffing](https://www.elastic.co/de/blog/elasticsearch-sniffing-best-practices-what-when-why-how)in den verwendeten Elasticsearch Clients zu deaktivieren (z.B. fluentd). Elastic Agent, Filebeat etc. verhalten sich automatisch richtig.
- Falls Sie Daten aus Ihrem bestehenden Elastic Cluster in Ihr neues Deployment migrieren möchten, finden Sie einige Möglichkeiten in der Elastic Dokumentation: [https://www.elastic.co/guide/en/cloud-enterprise/current/ece-migrating-data.html](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-migrating-data.html).
- Prüfen Sie in Kibana Ihre Index Templates und Lifecycle Policies, damit Sie Ihre Daten den von Ihnen gewünschten Tiers Hot, Warm, Cold, Frozen (und Delete) zuweisen können. Die von Elastic automatisch angelegten Lifecycle Policies haben häufig nur ein Hot Tier ohne Ablaufdatum konfiguriert. Weitere Informationen finden Sie hier: [https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html).
Dies gilt insbesondere für die standardmäßig deployten Lifecycle Policies **logs** und **metrics**, die bei Verwendung des Elastic Agents die relevantesten Policies sind.

Ein Beispiel für die Konfiguration einer Policy, bei der die Daten nach 70 Tagen gelöscht werden, finden Sie hier: [Aktualisieren der Standard Lifecycle Policies](/ece/updateilm/).

## Support-Leistungen von GEC

Bei Support-Anfragen können Sie sich an unseren Support wenden, der werktags von 8-18 Uhr erreichbar ist.

Der Support von GEC kann Sie bei allen Themen unterstützen, die im Rahmen des ECE-Dienstes in der Verantwortung von GEC liegen:

- Verfügbarkeit und Erreichbarkeit der Plattform
- Fragen zur Verwendung von ECE
- Fehler in der Elastic Software (wir würden diese entsprechend an Elastic weiterleiten)

Während wir daran arbeiten, Ihnen in Zukunft eine Self-Service-Oberfläche für die Verwaltung Ihres ECE-Deployments zur Verfügung zu stellen, müssen Sie vorläufig folgende Einstellungen an Ihrem Deployment über unseren Support beauftragen. Während dieser Zeit sind die Leistungen auch im Support-Umfang enthalten:

- Erweiterte Konfiguration des Deployments (d.h. Anpassungen an der elasticsearch.yml bzw. kibana.yml)
- Einträge im Elasticsearch Keystore
- Upgrade der Elastic-Version Ihres Deployments
- Konfiguration von IP-Filtern für Ihr Deployment
- Konfiguration von Vertrauensbeziehungen zwischen Deployments, damit Sie Cross-Cluster Search bzw. Cross-Cluster Replication verwenden können

**Nicht** durch den Support der GEC abgedeckt sind:

- Generelle Beratung zur Elastic Software oder deren Verwendung in Ihrem Deployment

Ggf. können wir auf Anfrage einige der nicht abgedeckten Leistungen als vergütete Consulting-Leistung anbieten. Bitte wenden Sie sich hierzu an den Vertrieb.

Ebenso können wir keine 24/7-Rufbereitschaft für Kunden anbieten. Dies ist auch nicht nötig, da die ordnungsgemäße Funktionalität der ECE-Plattform von einem automatisierten Monitoring überwacht wird. Wird eine Einschränkung des Service festgestellt, wird unsere interne Rufbereitschaft automatisch alarmiert und beginnt mit der Problembehebung.
