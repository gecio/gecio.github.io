---
title: About GKS
lang: en
permalink: /gks/about/
nav_order: 1000
has_children: true
---


## What is the GKS platform?

The GKS-platform offers **Managed Kubernetes Clusters**. As a customer, you can create, update and delete managed Kubernetes clusters with just a simple click in our Webinterface.

### What is a managed Kubernetes cluster?

Kubernetes clusters in general provide a reliable and scalable environment to run containerized applications.

With a managed Kubernetes cluster on our GKS-platform:

* We take care of the underlying infrastructure and the Kubernetes installation.
* We ensure scalability and provide you the option to configure the amount and size of your worker-nodes for your needs.
* We completely manage the controlplane for you.

While we ensure that the cluster itself is up and running, you have full access to the cluster via a `kubeconfig`-file. You have control over your clusters all of the time:

* You can easily create and manage an unlimited amount of Kubernetes clusters via our webinterface.
* You can even upgrade the Kubernetes version of existing clusters with just a click.
* You can of course instantly delete clusters which are not needed anymore.
* You can have full SSH-root access to your worker-nodes, if needed (i.e. for debugging your own applications).

And in case anything goes wrong, you can reach out to our [24/7 support](mailto:support@gec.io) or, as an optional service, schedule a session with our Professional Services team to deep-dive and discuss your technical questions and architecture.

### How can I use a managed Kubernetes cluster?

Of course you can use the GKS-clusters like any Kubernetes cluster - just with the GKS-platform, you can focus on your application instead of having to care about the underlying infrastructure.

For example, you could install a [pre-packaged containerized application](https://artifacthub.io/) using [helm](https://helm.sh/):

* Like a full [WordPress](https://artifacthub.io/packages/helm/bitnami/wordpress) installation,
* a [Drupal](https://artifacthub.io/packages/helm/bitnami/drupal) CMS,
* or your own private [Nextcloud](https://artifacthub.io/packages/helm/nextcloud/nextcloud) service.

Besides pre-packaged and ready-to-use applications, Kubernetes is also the natural environment to develop, build and run your own, state-of-the-art "cloud-native" applications.

As a development-environment, you could install the following apps to a Kubernetes cluster:

* You can install a [gitlab](https://artifacthub.io/packages/helm/gitlab/gitlab) to manage your sourcecode,
* automate your workflows using [Jenkins](https://artifacthub.io/packages/helm/jenkinsci/jenkins),
* and store your build results in [Artifactory](https://artifacthub.io/packages/helm/jfrog/artifactory).

And then maybe create dev, staging and production Kubernetes clusters using the GKS-platform and use easy-to-install, existing components to support your application architecture, such as:

* A complete [Kafka](https://artifacthub.io/packages/helm/bitnami/kafka) setup,
* a [PostgreSQL](https://artifacthub.io/packages/helm/bitnami/postgresql)-, [MySQL](https://artifacthub.io/packages/helm/bitnami/mysql)- or [MariaDB](https://artifacthub.io/packages/helm/bitnami/mariadb)-database,
* or a full-featured [Prometheus-Monitoring-Stack](https://artifacthub.io/packages/helm/prometheus-community/kube-prometheus-stack) incl. Prometheus, Alertmanager and Grafana.

But the above are just examples. If you want to deep dive and get consulting on how to best build your app on Kubernetes, be sure to [schedule a session with our Professional Services team](mailto:support@gec.io).

## How does the GKS platform work?

The GKS-platform itself is based on Kubernetes as well. Internally, GKS runs the controlplane of a customer cluster in a dedicated Kubernetes namespace with all required workloads running as deployments inside this namespace:

* Kubernetes API server
* Etcd database
* controller-manager
* scheduler
* machine-controller
* ...and other workloads.

Running the controlplane in Kubernetes has many advantages for management, such as automated scheduling as well as scaling and self-healing capabilities.

![GKS platform](gks-platform.png)

The worker-nodes of a customer cluster are dedicated VMs in the Openstack tenant of a customer. The machine-controller automatically creates VMs there and adds them as worker-nodes to the cluster. It scales the number of nodes up or down as required and will perform any changes to a Machine Deployment as a rolling upgrade to minimize downtime.

## Certifications

<img src="certified-kubernetes.png" alt="Certified Kubernetes Logo" width="100"/>

GKS is a product to efficiently setup and operate Kubernetes clusters on GEC Cloud.
Customers have the option to use a powerful webinterface to deploy and manage completely
functional clusters that also pass the conformance checks and requirements by
[Cloud Native Computing Foundation](https://cncf.io/ck).

Conformance results of our platform can be validated and checked at
[CNCF Kubernetes Conformance](https://github.com/cncf/k8s-conformance)
