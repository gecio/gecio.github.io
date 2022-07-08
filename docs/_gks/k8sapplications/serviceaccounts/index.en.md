---
title: Managing Service Accounts
lang: en
permalink: /gks/k8sapplications/serviceaccounts/
nav_order: 7200
parent: Kubernetes Applications
---

Limited access to a Kubernetes cluster can be achieved using Kubernetes service accounts, and the RBAC feature within Kubernetes.

To achieve this, we will need to create:

- Kubernetes service account
- access role
- role binding for the Kubernetes service account to use the access role

All authentication with Kubernetes clusters created in GKS is done using
bearer tokens. When you create a new Kubernetes service account a secret will
be created for it in the same namespace, and it will be automatically
removed when you delete the Kubernetes service account.

## Creating a Kubernetes service account

To create a Kubernetes service account run the following command and replace
`my-serviceaccount` with the name you want to use for the Kubernetes service
account:

```bash
kubectl apply -f - <<EOF
apiVersion: v1
kind: ServiceAccount
metadata:
  name: my-serviceaccount
  namespace: my-namespace
EOF
```

The cluster will then automatically create a new access token with a name
like `my-serviceaccount-token-#####` where the `#`s are alphanumeric characters.

To get a list of the tokens in a specific namespace, run:

```bash
kubectl get secrets --namespace=my-namespace
```

Then we can print the token we want to use with the following command and
replacing $SECRETNAME with the one that has been created for our service
account:

```bash
kubectl get secret $SECRETNAME -o jsonpath='{.data.token}' --namespace=my-namespace
```

Provide the token that has been printed with the name of the service account
to a developer or third party to allow them to interact with the cluster.

At this point the service account can authenticate with the Kubernetes
cluster, but is unable to use it to do anything. We now need to create a role,
and a role binding to provide permissions to the service account.

## Creating authorization permissions

Kubernetes has two ways of granting access to service accounts, roles and
cluster roles. As cluster roles provide access to all namespaces it is
recommended that you don't use them unless you have to. We will provide
examples on how to use the namespace restricted roles here.

Roles explicitly whitelist permissions for users (both human and Kubernetes
service accounts) and when a user has multiple roles they can do anything
that is granted by any of the roles.

To create a role to allow a user to read information about pods in the
namespace `my-namespace` we use the following command:

```bash
kubectl apply -f - <<EOF
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  namespace: my-namespace
  name: pod-reader
rules:
- apiGroups: [""] # "" indicates the core API group
  resources: ["pods"]
  verbs: ["get", "watch", "list"]
EOF
```

To grant the service account we created earlier to use this role, we use the
following command to create a _role binding_:

```bash
kubectl create rolebinding read-pods \
  --role=pod-reader \
  --serviceaccount=my-namespace:my-serviceaccount \
  --namespace=my-namespace
```

For a full list of resources run

```bash
kubectl api-resources
```

And for most resources the available verbs are:

- get
- list
- watch
- create
- edit
- update
- delete
- exec

### More information

The official kubernetes documentation on [controlling access](https://kubernetes.io/docs/reference/access-authn-authz/controlling-access/) and [using roles and role bindings in RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/) provides more details.

## Summary

In this guide we learned how to use the Kubernetes CLI to:

- Create a Kubernetes service account
- Retrieve the automatically generated bearer token for a service account
- Create a new role in Kubernetes RBAC
- Create a binding to allow the Kubernetes service account to use that role
