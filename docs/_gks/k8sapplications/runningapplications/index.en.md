---
title: Running Applications in Kubernetes
lang: "en"
permalink: /gks/k8sapplications/runningapplications/
nav_order: 8100
parent: Kubernetes Applications
---
# Running Applications in Kubernetes

You have a running cluster, and now you want to run an
application on it. In this example, you run
NGINX with a Load Balancer in front of the cluster.

## Prerequisites

To successfully finish the steps below, you need the following:

* `kubectl` [latest version](https://kubernetes.io/docs/tasks/tools/#kubectl)
* A running Kubernetes cluster created with GKS, with a ready Machine Deployment
  * For more information, see [Creating a Cluster](/gks/clusterlifecycle/creatingacluster).
* A valid `kubeconfig` for your cluster
  * For more information, see [Connecting to a Cluster](/gks/accessmanagement/connectingtoacluster).

## Data Types

Everything running in Kubernetes (inside the cluster) is tracked by the API server.
This is how you run and manage applications in Kubernetes.

### Deployment

The data type `deployment` ensures that an application runs in
Kubernetes and can be updated with a chosen method, for
example _Rolling_.

You need to create a `deployment` for your NGINX example, so Kubernetes
knows it needs to download a docker image that contains NGINX to the
cluster.

### Service

A service in Kubernetes is a collection of various containers
running in the cluster. The containers are matched to the
collection with labels which you apply to deployments.

A service can have several types. In our example, you choose
the type `LoadBalancer` to make your service accessible from
outside the cluster with a public IP address.

## Manifests

To run NGINX on Kubernetes, you need an object of type `deployment`.
You can easily create it with `kubectl`:

```bash
kubectl create deployment --dry-run -o yaml --image nginx nginx

apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```

You store this in a file called
`deployment.yaml`.

```bash
kubectl create deployment --dry-run -o yaml --image nginx nginx > deployment.yaml
```

Next, you need the service which makes the application publicly accessible.
As type you choose `LoadBalancer`. This automatically creates a fully
configured LoadBalancer in OpenStack, and allows you to access the cluster.

```bash
kubectl create service loadbalancer --dry-run --tcp=80 -o yaml nginx

apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: nginx
  name: nginx
spec:
  ports:
  - name: "80"
    port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx
  type: LoadBalancer
status:
  loadBalancer: {}
```

Again, you save this into a file. This time you name it `service.yaml`.

```bash
kubectl create service loadbalancer --dry-run --tcp=80 -o yaml nginx > service.yaml
```

These two files are the basis for a publicly accessible NGINX running on
Kubernetes. There is no connection between the two manifests, except for the
label `app: nginx`, which you can find in the deployment's metadata and in the
service as a selector.

## Creating the Application

To create the application, you send the two previously created files to the
Kubernetes API:

```bash
kubectl apply -f deployment.yaml
deployment.apps/nginx created

kubectl apply -f service.yaml
service/nginx created
```

Now you can inspect the two objects you created before as follows:

```bash
kubectl get deployment,service
NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/nginx   1/1     1            1           55s

NAME                 TYPE           CLUSTER-IP    EXTERNAL-IP   PORT(S)         AGE
service/kubernetes   NodePort       10.10.10.1    <none>        443:31630/TCP   2d23h
service/nginx        LoadBalancer   10.10.10.86   <pending>     80:31762/TCP    46s
```

As shown in the output, a deployment was created and is in the READY state.
The service NGINX was created as well, but EXTERNAL-IP is still pending. You need
to wait a minute until the LoadBalancer has started up completely.

A couple of minutes later, you can run the command again and now you see an
external IP address:

```bash
kubectl get deployment,svc
NAME                          READY   UP-TO-DATE   AVAILABLE   AGE
deployment.extensions/nginx   1/1     1            1           2m8s

NAME                 TYPE           CLUSTER-IP    EXTERNAL-IP       PORT(S)         AGE
service/kubernetes   NodePort       10.10.10.1    <none>            443:31630/TCP   2d23h
service/nginx        LoadBalancer   10.10.10.86   185.116.245.169   80:31762/TCP    119s
```

The external IP `185.116.245.169` from this example is now reachable from the
public internet and shows the NGINX default page.

## Cleanup

It's easy to clean up an application you do not want to run anymore.

```bash
kubectl delete -f service.yaml
service "nginx" deleted

kubectl delete -f deployment.yaml
deployment.apps "nginx" deleted

kubectl get deployment,svc

NAME                 TYPE       CLUSTER-IP   EXTERNAL-IP   PORT(S)         AGE
service/kubernetes   NodePort   10.10.10.1   <none>        443:31630/TCP   2d23h
```

As you can see, both deployment and service are deleted. If you try to open the website via IP again,
you will get an error: Our application has stopped running.

## Summary

You learned and achieved the following:

* How to talk to Kubernetes
* What is a deployment
* How to create a deployment
* What is a service
* How to create a service
* How to delete an application

Congratulations! You now know all required steps to start and delete applications in Kubernetes.
