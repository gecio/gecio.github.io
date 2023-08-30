---
title: Introduction
lang: "en"
permalink: /ece/intro
has_children: false
nav_order: 1000
---

# Introduction

Elastic Cloud Enterprise by German Edge Cloud (ECE) is a cloud platform hosted and operated by GEC, on which you can run your Elastic workloads in the form of deployments.

A deployment is an isolated Elastic Cluster managed by ECE. It consists of several components of the Elastic Stack. For each deployment you can specify the following individually:

- Which of the Elastic Stack components below to use
- The size of each component
- The number of Availability Zones (1 through 3) over which to distribute them
- Which of the GEC provided versions for the Elastic Stack to use

Each deployment gets its own publicly accessible URLs for Elasticsearch, Kibana, Fleet, and Enterprise Search.

## Components of the Elastic Stack

ECE includes all server components currently offered by Elastic:

- Elasticsearch in the 4 tiers Hot, Warm, Cold and Frozen as well as master nodes and coordinating/ingest nodes
- Kibana
- Machine Learning
- Integration Server (Fleet and APM)
- Enterprise Search

Information on the Elasticsearch data tiers is available here: [https://www.elastic.co/guide/en/elasticsearch/reference/current/data-tiers.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/data-tiers.html).

## Functional Scope of the Components

All ECE deployments are equipped with the Elastic Enterprise license, which means that all components contain all the functionalities of the highest-level Elastic license. This includes machine learning, searchable snapshots in the frozen tier (a very cost-efficient variant for storing rarely accessed data), cross-cluster search, all SIEM (Security Information and Event Management) functionalities of the Elastic Stack, Watcher and Alerting, the brand-new AI Assistant and much more.

You can find an overview of the components here: [https://www.elastic.co/de/pricing/](https://www.elastic.co/de/pricing/), and a detailed list here: [https://www.elastic.co/de/subscriptions](https://www.elastic.co/de/subscriptions).

## Calculation of Deployment Size and Pricing Model

The deployment size is measured in GB RAM. Each Elastic component is also allocated CPU and storage, derived from the RAM size by certain factors. This mapping factor can be different for each component. In most use cases, RAM is the main determining factor for the performance of elastic workloads. Since the mapping is based on fixed factors, one parameter (GB RAM) is sufficient to calculate the other parameters (CPU, storage). This makes the pricing model very transparent and easy to understand. All you need to know is the RAM size of the deployments.

Your cost is based on GB of RAM used in your deployment. Consumption is recorded and billed to the hour.

The snapshot storage usage in our object storage cluster is billed separately. Internet traffic (in and out) is free of charge.

## Autoscaling Feature

The Elasticsearch and machine learning components come with autoscaling enabled. You don't have this feature with a self-hosted Elastic Stack or you would have to develop it yourself. With autoscaling, ECE monitors the disk space used by the Elasticsearch components and automatically adds more resources when a threshold is exceeded. As a result, you do not have to worry about the otherwise very common case of a deployment outage due to lack of disk space, even when your data volume grows. Machine learning workloads are initiated at the proper size only when actually needed. The resources for your deployment and thus your costs grow with your data volume. This enables efficient use of resources and cost-effective operation of your workloads on the ECE platform.

For each component with autoscaling, an upper limit for scaling can be defined. This way, your invoice amount does not get out of control, e.g. in case of a misconfiguration.

## Your Advantages with ECE from GEC

With the ECE solution from GEC you get all the functionalities of the software from Elastic, the market leader in search, SIEM and observability, including the enterprise license. GEC hosts exclusively in highly secure German data centers and carries out all operational tasks from Germany. You get everything from one source from a German contractual partner. In contrast to a "classic" deployment of the Elastic Stack in a cluster with virtual machines, which are limited to the sizes specified by the respective cloud platform, ECE uses Docker containers. These can scale automatically and quickly, if necessary. Fine-grained autoscaling allows you to use resources efficiently and run workloads cost-effectively.

## Services Offered by GEC

- GEC provides the cloud environment on which your Elastic workloads run in its secure and certified data centers in Germany.
- GEC allows you to distribute your workloads across 3 Availability Zones for high availability.
- GEC takes care that enough resources are available, in case your deployment needs to scale up.
- GEC monitors the ECE environment and ensures that it is available (99.85 %). Outside office hours, an on-call service is automatically alerted to solve unforeseen problems with the ECE platform as quickly as possible.
- GEC ensures regular updates and security patches for the servers.
- GEC makes new versions of the Elastic Stack available for installation, typically only a few days after Elastic releases them. However, we do not perform automatic or unsolicited version updates of your deployment (except for EOL versions).
- GEC provides URLs that can be reached from the Internet for all deployments, including a TLS certificate for encrypted communication (customer-specific certificates are unfortunately not possible).
- GEC offers backup and snapshot storage in the redundant GEC Object Storage Cluster. By default, each deployment has a snapshot policy enabled (which you can change) that automatically backs up your Elasticsearch data to the object storage at regular intervals. This protects you very well against data loss.
- All your deployments in the GEC ECE cloud come with the Elastic Enterprise license. You don't have to buy licenses from Elastic. GEC provides you with everything from a single source.

## Your Responsibilities as a Customer

As a customer, you are responsible for everything that happens **within** your deployment, in particular:

- Collecting data from client systems and loading that data into Elasticsearch
- The configuration of Lifecycle Policies
- The creation of users and the assignment of authorizations
- Monitoring shard health and repairing it if necessary (for more information, see the [elastic.co](https://www.elastic.co/) documentation)
- Configuration of alerts - if desired
- Restore snapshots when needed
- Monitoring the performance of your deployment if you have special requirements for this. We cannot know which performance is acceptable for your application or use case.

More provisions can be found in the General Terms and Conditions of GEC.

## Service Description

For more information, please contact Sales.

## Steps To Be Performed After the ECE Deployment

We recommend performing the following steps to increase the security of your ECE deployment and load data into Elastic:

### Increasing Security

- Create the required spaces, roles and users in Kibana. For more information, see: [https://www.elastic.co/guide/en/kibana/current/tutorial-secure-access-to-kibana.html](https://www.elastic.co/guide/en/kibana/current/tutorial-secure-access-to-kibana.html).
- Do not use the elastic superuser for daily work in Kibana or to deliver data.
- If desired, configure Single Sign-On using [SAML](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-securing-clusters-SAML.html), [LDAP](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-securing-clusters-ldap.html), [Active Directory](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-securing-clusters-ad.html), [OpenID Connect](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-secure-clusters-oidc.html) or [Kerberos](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-secure-clusters-kerberos.html) as well as dynamic [role mappings](https://www.elastic.co/guide/en/elasticsearch/reference/current/mapping-roles.html), if you need role-based or attribute-based access control. For the time being, to set those parameters in your deployment, contact our support.
- If desired, have our support configure [traffic filters](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-traffic-filtering-deployment-configuration.html). This prevents unwanted access to your deployment.
- Create service accounts for data delivery (when not using Agent and Fleet, cf. below) and generate API keys for them. For more information see: [Grant access using API keys](https://www.elastic.co/guide/en/beats/filebeat/current/beats-api-keys.html).

### Loading Data into Elastic

- Load data into your cluster. We recommend using the Elastic Agent in cooperation with the Fleet Server. You can find detailed instructions here: [Ship Data to ECE](/ece/shipdata).
- To avoid connection problems to your cluster, make sure to disable [sniffing](https://www.elastic.co/de/blog/elasticsearch-sniffing-best-practices-what-when-why-how) in the Elasticsearch clients used (e.g. fluentd). Elastic Agent, Filebeat etc. automatically behave correctly.
- If you want to migrate data from your existing Elastic cluster to your new deployment, you can find some methods in the Elastic documentation: [https://www.elastic.co/guide/en/cloud-enterprise/current/ece-migrating-data.html](https://www.elastic.co/guide/en/cloud-enterprise/current/ece-migrating-data.html).
- In Kibana, review your index templates and lifecycle policies so you can assign your data to your desired Hot, Warm, Cold, Frozen (and delete) tiers. The lifecycle policies automatically created by Elastic often only have one hot tier configured without an expiry date (see: [https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-lifecycle-management.html)). This applies in particular to the automatically deployed lifecycle policies **logs** and **metrics**, which are the most relevant policies when using the Elastic Agent.
For an example of configuring a policy that deletes data after 70 days, see: [Update the "Default" ILM](/ece/updatedata/).

## Support Services by GEC

For support requests, you can contact our support team, which is available on workdays from 8 a.m. to 6 p.m.

The GEC Support can assist you with all topics that are GEC's responsibility within the scope of the ECE service:

- Availability and accessibility of the platform
- Questions about using ECE
- Bugs in the Elastic software (we would forward these to Elastic accordingly)

While we are working on providing you with a self-service interface for managing your ECE deployment in the future, you must initially request the following settings for your deployment from our support. During this time, the services are also included in the scope of support:

- Extended configuration of the deployment (i.e. adjustments to the elasticsearch.yml or kibana.yml)
- Entries in the Elasticsearch keystore
- Update of the Elastic version of your deployment
- Configuration of traffic filters for your deployment
- Configuration of trust relationships between deployments so you can use cross-cluster search or cross cluster replication

The following is **not** covered by GEC Support:

- General consulting about the Elastic software or using it in your deployment

If applicable, upon request we may offer some of the uncovered services as paid consulting services. Please contact Sales, if needed.

Likewise, we cannot offer 24/7 on-call service for customers. This should also not be necessary, since the proper functionality of the ECE platform is controlled by automated monitoring. If a restriction of the service is noticed, our internal on-call service is automatically alerted and starts rectifying the issue.
