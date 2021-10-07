---
title: Add ssh key to an existing cluster
lang: en
permalink: /imke/machinedeployments/add_ssh_key/
nav_order: 5400
parent: Machine Deployments
---

The iMKE-platform offers the possibility to add SSH keys to worker nodes.
This could be useful if you want to debug your Kubernetes clusters and your application directly from the worker nodes.

To achieve this, you will need to:

- Create a ssh key,
- Have a cluster with `User SSH Key Agent` enabled,
- Add the key to the project and
- Enable it in the Cluster.

In most cases you also need to assign a Floating IP to your worker nodes to be able to access them.

## User SSH Key Agent

In order to be able to manage SSH keys on the worker nodes, the `User SSH Key Agent` must be enabled during cluster creation:

![User SSH Key Agent during Create cluster](user-ssh-key-agent-create.png)

If you do not activate this setting during cluster creation, you cannot add/modify SSH-keys later on. Please be advised that the
User SSH Key Agent can only be added during cluster creation. If you didn't enable the User SSH Key Agent during creation, you cannot enable it later on.

### Checking the Status of the User SSH Key Agent

To check if the User SSH Key Agent is enabled for a certain cluster, you can check the cluster status page:

![User SSH Key Agent status](user-ssh-key-agent-status.png)

If the Agent is activated, SSH-Keys can be added at any time like described below.

### Other ways of managing User SSH Keys

It is possible to create a cluster **without** enabling the User SSH Key Agent. In this case all worker nodes will be created without any SSH-keys added - and SSH-keys cannot be changed using our platform. This would allow other methods/agents of managing SSH keys like saltstack, puppet or chef if the worker-nodes are created from a custom image supporting this. It is not possible to add the User SSH Key Agent
after cluster creation to not interfere and accidentially break such setups.

## Adding an SSH Key to an existing cluster

If you want to add an SSH Key to an existing cluster which has the User SSH Key Agent is enabled, you can follow the below steps.

### Creating an ssh key

The simplest way to generate a key pair is to run `ssh-keygen` without arguments:

```bash
ssh-keygen
```

A SSH key will be created. The default path for the ssh key is: `~/.ssh/id_rsa.pub`.

### Add the ssh key to the project

1. Select the project:

    ![Projects](projects.png)

2. Go to the SSH Key page:

    ![Project-Menu](project-menu.png)

3. Use the `Add SSH Key` button:

    ![SSH-Key-Page](ssh-key-page.png)

4. Name the key and paste the public SSH key which was created by `ssh-keygen` (not the private key!):

    ![Ssh-key](ssh-key.png)

Now you can use the key in any cluster in this project.

### Add the ssh key to the cluster

1. Select a cluster where you want to add the key:

    ![Cluster](clusters.png)

2. Click the three dots, to open the cluster sub menu:

    ![Three-Dots](three-dots.png)

3. Select `Manage SSH keys`:

    ![Edit-Cluster](manage-ssh-keys.png)

4. Now the newly created SSH key can be selected from a drop-down list.

    ![Manage-Keys](manage-keys.png)

5. After the selection, the key is displayed in the list and can also be deleted from it if required.

    ![Key-List](key-list.png)

Your Key will now be added to all worker nodes in all machinedeployments.

## Adding an SSH Key during cluster creation

It is also possible to add SSH keys during cluster creation. 
How to achieve this is described in the [Creating a Cluster](/imke/clusterlifecycle/creatingacluster/)-section of our documentation.

## Access to the node

Once you added the SSH key to the cluster, you need to attach a Floating IP to the `Machine Deployment` to be able access the worker nodes via SSH.

To achieve this, you have edit the `Machine Deployment`:

![Edit-MD](edit_machine_deployment.png)

And ensure `Allocate Floating IP` is selected:

![Enable-Floating_IP](enable-fip.png)

Once the node is fully created, and has an external ip, you can access to the node using the key.
The default user for Ubuntu is `ubuntu` and for Flatcar `core`.

```bash
 ssh -A ubuntu@PUBLIC_IP
 ssh -A core@PUBLIC_IP
```
