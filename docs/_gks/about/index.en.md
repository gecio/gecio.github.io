---
title: About GKS
lang: "en"
permalink: /gks/about/
nav_order: 1000
has_children: true
---

# About GKS

## What Is the GKS Platform?

The GKS platform offers **Managed Kubernetes Clusters**. As a customer, you can create, update, and delete managed Kubernetes clusters with just a simple click in our web interface.

### What Is a Managed Kubernetes Cluster?

Kubernetes clusters in general provide a reliable and scalable environment to run containerized applications.

With a managed Kubernetes cluster on our GKS platform, we:

* Take care of the underlying infrastructure and the Kubernetes installation
* Ensure scalability and provide you the option to configure the amount and size of your worker nodes for your needs
* Completely manage the control plane for you

While we ensure that the cluster itself is up and running, you have full access to the cluster with a `kubeconfig`-file. You have control over your clusters all time.

You can:

* Easily create and manage an unlimited amount of Kubernetes clusters with our web interface.
* Even upgrade the Kubernetes version of existing clusters with just a click.
* Instantly delete clusters that are not needed anymore.
* Have full SSH-root access to your worker nodes, if needed (for example, for debugging your own applications).

In case anything goes wrong, you can reach out to our [24/7 support](mailto:support@gec.io) or, as an optional service, schedule a session with our Professional Services team to deep-dive and discuss your technical questions and architecture.

### How Can I Use a Managed Kubernetes Cluster?

Of course you can use the GKS clusters like any Kubernetes cluster - just with the GKS platform, you can focus on your application instead of having to care about the underlying infrastructure.

For example, you can install a [pre-packaged containerized application](https://artifacthub.io/) with [helm](https://helm.sh/), such as:

* A full [WordPress](https://artifacthub.io/packages/helm/bitnami/wordpress) installation
* A [Drupal](https://artifacthub.io/packages/helm/bitnami/drupal) CMS
* Your own private [Nextcloud](https://artifacthub.io/packages/helm/nextcloud/nextcloud) service

Besides pre-packaged and ready-to-use applications, Kubernetes is also the natural environment to develop, build, and run your own, state-of-the-art "cloud-native" applications.

For a development-environment, you can install the following apps to a Kubernetes cluster:

* [gitlab](https://artifacthub.io/packages/helm/gitlab/gitlab) to manage your sourcecode
* [Jenkins](https://artifacthub.io/packages/helm/jenkinsci/jenkins) to automate your workflows
* [Artifactory](https://artifacthub.io/packages/helm/jfrog/artifactory) to store your build results in

Then you may create dev, staging, and production Kubernetes clusters with the GKS platform and use easy-to-install, existing components to support your application architecture, such as a:

* Complete [Kafka](https://artifacthub.io/packages/helm/bitnami/kafka) setup
* [PostgreSQL](https://artifacthub.io/packages/helm/bitnami/postgresql)-, [MySQL](https://artifacthub.io/packages/helm/bitnami/mysql)- or [MariaDB](https://artifacthub.io/packages/helm/bitnami/mariadb)-database
* Full-featured [Prometheus-Monitoring-Stack](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack) incl. Prometheus, Alertmanager, and Grafana

These are just examples. If you want to deep dive and get consulting on how to best build your app on Kubernetes, [schedule a session with our Professional Services team](mailto:support@gec.io).

## How Does the GKS Platform Work?

The GKS platform itself is based on Kubernetes as well. Internally, GKS runs the control plane of a customer cluster in a dedicated Kubernetes namespace with all required workloads running as deployments inside this namespace:

* Kubernetes API server
* Etcd database
* Controller-manager
* Scheduler
* Machine-controller
* Other workloads

Running the control plane in Kubernetes has many advantages for management, such as automated scheduling as well as scaling and self-healing capabilities.

![GKS platform](gks-platform.png)

The worker nodes of a customer cluster are dedicated VMs in the Openstack tenant of a customer. The machine-controller automatically creates VMs there and adds them as worker nodes to the cluster. It scales the number of nodes up or down as required and performs any changes to a machine deployment as a rolling upgrade to minimize downtime.

## Certifications

<img src="certified-kubernetes.png" alt="Certified Kubernetes Logo" width="100"/>

GKS is a product to efficiently set up and operate Kubernetes clusters on GEC Cloud.
Customers have the option to use a powerful web interface to deploy and manage completely
functional clusters that also pass the conformance checks and requirements by the
[Cloud Native Computing Foundation](https://cncf.io/ck).

Conformance results of our platform can be validated and checked at
[CNCF Kubernetes Conformance](https://github.com/cncf/k8s-conformance)
