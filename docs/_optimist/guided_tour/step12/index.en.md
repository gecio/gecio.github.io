---
title: "12: A usable VM"
lang: en
permalink: /optimist/guided_tour/step12/
nav_order: 1120
parent: Guided Tour
---

# Step 12: A usable VM

## Start

Although you already created a VM in [Step 7](/optimist/guided_tour/step07/), the VM was not usable as it was not connected to a network, let alone the internet. Let's create one that you can actually log on to.

## Installation

To create this VM, add some parameters to the command we used in [Step 7](/optimist/guided_tour/step07/):

```bash
$ openstack server create BeispielInstanz --flavor m1.small --key-name Beispiel --image "Ubuntu 16.04 Xenial Xerus - Latest" --security-group allow-ssh-from-anywhere --network=BeispielNetzwerk
+-----------------------------+---------------------------------------------------------------------------+
| Field                       | Value                                                                     |
+-----------------------------+---------------------------------------------------------------------------+
| OS-DCF:diskConfig           | MANUAL                                                                    |
| OS-EXT-AZ:availability_zone | es1                                                                       |
| OS-EXT-STS:power_state      | NOSTATE                                                                   |
| OS-EXT-STS:task_state       | scheduling                                                                |
| OS-EXT-STS:vm_state         | building                                                                  |
| OS-SRV-USG:launched_at      | None                                                                      |
| OS-SRV-USG:terminated_at    | None                                                                      |
| accessIPv4                  |                                                                           |
| accessIPv6                  |                                                                           |
| addresses                   |                                                                           |
| config_drive                |                                                                           |
| created                     | 2017-12-08T12:52:37Z                                                      |
| flavor                      | m1.small (b7c4fa0b-7960-4311-a86b-507dbf58e8ac)                           |
| hostId                      |                                                                           |
| id                          | 1de98aa4-7d2b-4427-a8a5-d369ea8bdaf5                                      |
| image                       | Ubuntu 16.04 Xenial Xerus - Latest (82242d21-d990-4fc2-92a5-c7bd7820e790) |
| key_name                    | Beispiel                                                                  |
| name                        | BeispielInstanz                                                           |
| progress                    | 0                                                                         |
| project_id                  | b15cde70d85749689e08106f973bb002                                          |
| properties                  |                                                                           |
| security_groups             | name='allow-ssh-from-anywhere'                                            |
| status                      | BUILD                                                                     |
| updated                     | 2017-12-08T12:52:37Z                                                      |
| user_id                     | 9bf501f4c3d14b7eb0f1443efe80f656                                          |
| volumes_attached            |                                                                           |
+-----------------------------+---------------------------------------------------------------------------+
```

The following parameters are included:

- `--flavor`: The flavor of the the VM. You get all available
    flavors with `openstack flavor list`.
- `--key-name`: The key to install on the VM.
- `--image`: The operating system image to install on the VM. You can
    get all available images with `openstack image list`.
- `--security-group`: Specifies the security group.
- `--network`: Specifies the network to attach the VM to.

If you want to reach your VM from the internet, you need a floating IP address.

You can create one as follows:

```bash
$ openstack floating ip create provider
+---------------------+--------------------------------------+
| Field               | Value                                |
+---------------------+--------------------------------------+
| created_at          | 2017-12-08T12:53:37Z                 |
| description         |                                      |
| fixed_ip_address    | None                                 |
| floating_ip_address | 185.116.245.65                       |
| floating_network_id | 54258498-a513-47da-9369-1a644e4be692 |
| id                  | 84eca140-9ac1-42c3-baf6-860ba920a23c |
| name                | None                                 |
| port_id             | None                                 |
| project_id          | b15cde70d85749689e08106f973bb002     |
| revision_number     | 0                                    |
| router_id           | None                                 |
| status              | DOWN                                 |
| updated_at          | 2017-12-08T12:53:37Z                 |
+---------------------+--------------------------------------+
```

The created IP must be associated with your VM:

```bash
openstack server add floating ip BeispielInstanz 185.116.245.145
```

## Usage

You should now have a reachable VM.

To check if all worked correctly, log in to your VM with SSH.

**IMPORTANT**: You can only log in if the specified ssh key exists and is
accessible (if it does not work, follow the instructions in [Step 6](/optimist/guided_tour/step06/)).

```bash
$ ssh ubuntu@185.116.245.145
The authenticity of host '185.116.245.145 (185.116.245.145)' can't be established.
ECDSA key fingerprint is SHA256:kbSkm8eJA0748911RkbWK2/pBVQOjJBASD1oOOXalk.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '185.116.245.145' (ECDSA) to the list of known hosts.
Enter passphrase for key '/Users/ubuntu/.ssh/id_rsa':
```

## Clean-Up

If you want to delete all components you just created, you must delete them
in the following order.

If you do not delete them in the correct order, you will be unable to delete
components that other resources depend on.

- Instance
  - `openstack server delete BeispielInstanz`
- Floating-IP
  - `openstack floating ip delete 185.116.245.145`
- Router Port
  - `openstack port delete BeispielPort`
- Router
  - `openstack router delete BeispielRouter`
- Subnet
  - `openstack subnet delete BeispielSubnet`
- Network
  - `openstack network delete BeispielNetzwerk`

## Conclusion

You have now created a VM based on your knowledge from steps 7 to 11, you can reach it from the internet, and have logged in with SSH.

In the next step, you will break away from individual instances and will create a stack.
