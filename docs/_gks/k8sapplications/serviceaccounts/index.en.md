---
title: Managing Service Accounts
lang: en
permalink: /gks/k8sapplications/serviceaccounts/
nav_order: 8200
parent: Kubernetes Applications
---

# Managing Service Accounts

Limited access to a Kubernetes cluster can be achieved using Kubernetes service accounts, and the RBAC feature within Kubernetes.

To achieve this, you need to create:

- A Kubernetes service account
- An access role
- Role binding for the Kubernetes service account to use the access role

All authentication with Kubernetes clusters created in GKS is done with
bearer tokens. When you create a new Kubernetes service account a secret will
be created for it in the same namespace, which will be automatically
removed when you delete the Kubernetes service account.

## Creating a Kubernetes Service Account

To create a Kubernetes service account, run the following command and replace
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

The cluster then automatically creates a new access token with a name
like `my-serviceaccount-token-#####` where the `#`s are alphanumeric characters.

To get a list of the tokens in a specific namespace, run:

```bash
kubectl get secrets --namespace=my-namespace
```

Then you can print the token you want to use with the following command and
replace $SECRETNAME with the one that has been created for your service
account:

```bash
kubectl get secret $SECRETNAME -o jsonpath='{.data.token}' --namespace=my-namespace
```

Provide the token that was printed with the name of the service account
to a developer or third party to allow them to interact with the cluster.

At this point the service account can authenticate with the Kubernetes
cluster, but is unable to use it. You need to create a role
and a role binding to provide permissions to the service account.

## Creating Authorization Permissions

Kubernetes has two ways of granting access to service accounts, roles, and
cluster roles. Since cluster roles provide access to all namespaces, it is
recommended not to use them unless you have to. We provide
examples on how to use the namespace-restricted roles here.

Roles explicitly whitelist permissions for users (both human and Kubernetes
service accounts). When users have multiple roles they can do anything
that is granted by any of the roles.

To create a role, which allows a user to read information about pods in the
namespace `my-namespace`, use the following command:

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

To grant the service account you created earlier to use this role, use the
following command to create a _role binding_:

```bash
kubectl create rolebinding read-pods \
  --role=pod-reader \
  --serviceaccount=my-namespace:my-serviceaccount \
  --namespace=my-namespace
```

For a full list of resources run:

```bash
kubectl api-resources
```

For most resources, the available verbs are:

- get
- list
- watch
- create
- edit
- update
- delete
- exec

### More Information

More details are available in the official Kubernetes documentation on [controlling access](https://kubernetes.io/docs/reference/access-authn-authz/controlling-access/) and [using roles and role bindings in RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/).

## Summary

In this section you learned how to use the Kubernetes CLI to:

- Create a Kubernetes service account
- Retrieve the automatically generated bearer token for a service account
- Create a new role in Kubernetes RBAC
- Create a role binding to allow the Kubernetes service account to use that role
