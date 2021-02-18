---
title: Role-Based Access Control (RBAC)
lang: en
permalink: /imke/accessmanagement/usingrbac/
nav_order: 2
parent: Access Management
---

To grant a user access via RBAC, expand the RBAC-widget and hit `Add Binding`:
![RBAC Add Binding](rbac_add.png)

### Cluster-wide permissions

To grant users cluster-wide permissions, leave the switch on `Cluster`, add the email of the user and select the role for the user:
![Add a cluserrolebinding](add_binding_cluster.png)

Please note that the user must exist in iMKE, or otherwise he/she will not be able to log in to download the kubeconfig later on.
The selectable Roles are predefined `ClusterRoles` which can be viewed by running `kubectl`:

```bash
kubectl get clusterrole $NAME_OF_CLUSTERROLE -o yaml
```

### Namespace-wide permissions

When access shall be granted on a namespace-level, switch to `Namespace` and add the user email there.

First you have to select the role which should be assigned to the user:
![Add a rolebinding #1](add_binding_ns_role.png)

Finally, you need to select the namespace where this should be valid:
![Add a rolebinding #2](add_binding_ns_namespace.png)

In case you want to see and understand the level of access granted here, you can view the mentioned roles via `kubectl` as well. Unlike `ClusterRoles`, `Roles` are scoped to a namespace, so you have to specify the namespace as well:

```bash
kubectl get role $NAME_OF_ROLE -n $NAMESPACE -o yaml
```

After you completed these steps, the rights should be visible in the RBAC widget of the Dashboard:
![RBAC option](rbac.png)

## Provide users with their kubeconfig

Once you assigned the user a cluster- oder namespace-wide role, you can provide him/her with a link to download the kubeconfig.

To do so, hit the `Share kubeconfig` button on the top of the Dashboard:
![Share kubeconfig button](share_kubeconfig.png)

Next, copy the link and send to the user:
![Share kubeconfig dialog](share_kubeconfig_dialog.png)

After the user has logged in, the download will start of the kubeconfig will start directly:
![Login page](login.png)

Once a user has downloaded his/her kubeconfig, any further changes made on the RBAC will have *immediate* effect. Especially there is no need to revoke cluster tokens to remove access for a user. Just remove his/her RoleBindings and access is no longer possible.
