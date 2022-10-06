---
title: Über GKS
lang: "de"
permalink: /gks/about/
nav_order: 1000
has_children: true
---
<!-- LTeX:  language=de-DE -->

# Über GKS

## Was ist die GKS Plattform?

Die GKS Plattform stellt **Managed Kubernetes Cluster** bereit. Als Kunde können Sie mit einem einfachen Klick in unserem Web Interface Kubernetes Cluster erstellen, aktualisieren oder auch wieder löschen.

### Was ist ein "Managed Kubernetes Cluster"?

Kubernetes Cluster bieten eine verfügbare und skalierbare Umgebung für containerisierte Anwendungen.

Bei einem gemanagten Kubernetes Cluster auf unserer GKS Plattform:

* Kümmern wir uns um die zugrundeliegende Server-Infrastruktur und die Kubernetes-Installation.
* Stellen wir die Skalierbarkeit Ihres Clusters sicher und geben Ihnen die Möglichkeit, die Anzahl und Größe Ihrer Worker-Nodes nach Ihren Bedürfnissen frei zu konfigurieren.
* Übernehmen wir das Management und den Betrieb der Kubernetes-Controlplane.

Während wir uns um den stabilen Betrieb Ihres Clusters kümmern, haben Sie trotzdem den Vollzugriff auf den Cluster über eine `kubeconfig`-Datei. Sie haben die volle Kontrolle über Ihre Cluster und können:

* Eine unbegrenzte Anzahl von Clustern über das GKS Web Interface verwalten und neue Cluster schnell erstellen.
* Die Kubernetes-Version bestehender Cluster mit einem einfachen Klick aktualisieren und so Ihre Cluster immer aktuell halten.
* Nicht mehr benötigte Cluster mit einem einfachen Klick löschen.
* Bei Bedarf vollen Root-Zugriff auf Ihre Worker-Nodes bekommen (zum Beispiel, wenn Sie Ihre selbst entwickelten Applikationen intensiv debuggen wollen).

Bei Problemen im Clusterbetrieb können Sie sich jederzeit an unseren [24/7-Support](mailto:support@gec.io) wenden. Jenseits der Betriebsleistung der GKS Plattform bieten wir mit unserem Professional Service Team auch eine weitergehende Beratung an. Wir helfen Ihnen beispielsweise bei allgemeinen Fragen zum Applikationsbetrieb auf Kubernetes oder auch bei spezifischen Fragen zum optimalen Anwendungs- und Softwaredesign für Ihr konkretes Vorhaben.

### Wie kann ich einen "Managed Kubernetes Cluster" benutzen?

Sie können einen GKS Cluster so benutzen wie jeden anderen Kubernetes Cluster auch. Da es sich bei einem GKS Cluster um einen gemanagten Kubernetes Cluster handelt, können Sie sich dabei voll und ganz auf den Applikationsbetrieb konzentrieren.

Beispielsweise können Sie ein GKS Cluster nutzen, um bereits [paketierte, containerisierte Anwendungen](https://artifacthub.io/) mit Hilfe von [helm](https://helm.sh/) zu installieren, zum Beispiel:

* Eine vollständige [WordPress-Installation](https://artifacthub.io/packages/helm/bitnami/wordpress)
* Ein [Drupal](https://artifacthub.io/packages/helm/bitnami/drupal) CMS
* Eine private [nextcloud](https://artifacthub.io/packages/helm/nextcloud/nextcloud) Filesharing-Umgebung

Kubernetes ist dabei selbstverständlich nicht auf bereits fertig paketierte, vollständige Anwendungen beschränkt. Vielmehr ist Kubernetes die natürliche Umgebung, um Ihre eigenen, zeitgemäßen "cloud-native"-Anwendungen zu entwickeln und produktiv zu betreiben.

Als Entwicklungsumgebung könnten Sie beispielsweise die folgenden Anwendungen in einem Kubernetes Cluster bereitstellen:

* [GitLab](https://artifacthub.io/packages/helm/gitlab/gitlab) zur Verwaltung des Sourcecode
* [Jenkins](https://artifacthub.io/packages/helm/jenkinsci/jenkins) zur Automatisierung Ihrer Entwicklungsworkflows
* [Artifactory](https://artifacthub.io/packages/helm/jfrog/artifactory) um Ihre Buildergebnisse versioniert abzulegen

Weiterhin könnten Sie jeweils einen Entwicklungs-, Staging- und Produktions-Cluster in GKS anlegen und Ihre Anwendungsarchitektur mit einfach zu installierenden und mit Helm paketierten Komponenten unterstützen, zum Beispiel:

* Einem kompletten [Kafka](https://artifacthub.io/packages/helm/bitnami/kafka)-Setup
* Einer [PostgreSQL](https://artifacthub.io/packages/helm/bitnami/postgresql)-, [MySQL](https://artifacthub.io/packages/helm/bitnami/mysql)- oder [MariaDB](https://artifacthub.io/packages/helm/bitnami/mariadb)-Datenbank
* Einem kompletten [Prometheus-Monitoring-Stack](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack) inkl. Prometheus, Alertmanager und Grafana

Dies sind nur einige Beispiele. Für eine weitergehende Beratung fragen Sie am besten direkt unser [Professional Services Team](mailto:support@gec.io).

## Wie funktioniert die GKS Plattform?

Die GKS Plattform selbst basiert ebenfalls auf Kubernetes. Intern läuft die Controlplane eines Kundenclusters in einem dedizierten Kubernetes Namespace. Die einzelnen Komponenten der Controlplane sind dabei als Deployments bzw. Statefulsets gestartet:

* Kubernetes API Server
* Etcd Datenbank
* Controller-Manager
* Scheduler
* Machine-Controller
* ... andere Komponenten der Controlplane.

Die Controlplane in Kubernetes zu betreiben, hat viele Vorteile: einzelne Pods werden automatisch über verschiedene Server verteilt (Scheduling), das gesamte Setup ist skalierbar und weitgehend selbstheilend, da abgestürzte Pods automatisch neugestartet werden.

![GKS platform](gks-platform.png)

Die Worker-Nodes eines Kundenclusters werden wiederum als selbstständige VMs im Openstack-Tenant des Kunden betrieben. Der Machine-Controller in der Controlplane kümmert sich dabei um die automatische Erstellung der VMs sowie ihre Eingliederung in den bestehenden Cluster. Dabei können mit dem Web Interface jederzeit neue Nodes im Cluster hinzugefügt oder auch gelöscht werden. Komplexere Änderungen werden durch den Machine-Controller in einem "Rolling-Upgrade" durchgeführt - immer eine Worker-Node nach der anderen. So werden Downtimes während eines Upgrades minimiert.

## Zertifizierungen

<img src="certified-kubernetes.png" alt="Certified Kubernetes Logo" width="100"/>

GKS ist ein Produkt um effizient Kubernetes Cluster in der GEC Cloud zu betreiben.
Kunden können über ein übersichtliches Web Interface mit nur ein paar Klicks komplett funktionale
und von der [Cloud Native Computing Foundation](https://cncf.io/ck)
zertifizierte Cluster hochfahren und für Applikations-Deployments verwenden.

Conformance Ergebnisse für unsere Plattform können Sie jederzeit unter
[CNCF Kubernetes Conformance](https://github.com/cncf/k8s-conformance)
einsehen.
