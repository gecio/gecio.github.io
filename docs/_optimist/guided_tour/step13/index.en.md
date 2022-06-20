---
title: "13: The structured way to create an instance (with stacks)"
lang: en
permalink: /optimist/guided_tour/step13/
nav_order: 1130
parent: Guided Tour
---

# Step 13: The structured way to create an instance (with stacks)

## Start

Previously, you created a VM, a security group, and a virtual network separately.

Now you will learn how to create all of them at once in an integrated way. This requires a pre-installed *python-heatclient*, which you was already installed in [Step 4: Our way to the console](/optimist/guided_tour/step04/).

## Installation

Instead of creating individual manually, any OpenStack resources (e.g. instances, networks, routers, security groups) can also be operated in a defined network; a *stack* (or heat stack).

This makes it easy to compose an entire setup, which you can then easily create
and delete at will.

In this step, you will use a pre-made heat template and later, will learn how to
write one yourself.

Everything you created in steps 9 through 11 are easily expressed in a
single template.

Let's start with an [example template.](https://github.com/innovocloud/openstack_examples/tree/master/heat/templates)

This template creates a stack that includes a VM, two security groups, a
virtual network (including router, port, and subnet), and a floating-IP.

When you create the stack, make sure that you are in the same directory as
the template:

```bash
$ openstack stack create -t SingleServer.yaml --parameter key_name=Beispiel SingleServer --wait
2017-12-08 13:13:43Z [SingleServer]: CREATE_IN_PROGRESS  Stack CREATE started
2017-12-08 13:13:44Z [SingleServer.router]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:45Z [SingleServer.enable_traffic]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:45Z [SingleServer.enable_traffic]: CREATE_COMPLETE  state changed
2017-12-08 13:13:46Z [SingleServer.internal_network_id]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:46Z [SingleServer.router]: CREATE_COMPLETE  state changed
2017-12-08 13:13:46Z [SingleServer.internal_network_id]: CREATE_COMPLETE  state changed
2017-12-08 13:13:47Z [SingleServer.enable_ssh]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:47Z [SingleServer.subnet]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:47Z [SingleServer.enable_ssh]: CREATE_COMPLETE  state changed
2017-12-08 13:13:48Z [SingleServer.start-config]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:48Z [SingleServer.subnet]: CREATE_COMPLETE  state changed
2017-12-08 13:13:49Z [SingleServer.router_subnet_bridge]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:49Z [SingleServer.port]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:50Z [SingleServer.start-config]: CREATE_COMPLETE  state changed
2017-12-08 13:13:50Z [SingleServer.port]: CREATE_COMPLETE  state changed
2017-12-08 13:13:50Z [SingleServer.host]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:52Z [SingleServer.router_subnet_bridge]: CREATE_COMPLETE  state changed
2017-12-08 13:13:53Z [SingleServer.floating_ip]: CREATE_IN_PROGRESS  state changed
2017-12-08 13:13:55Z [SingleServer.floating_ip]: CREATE_COMPLETE  state changed
2017-12-08 13:14:05Z [SingleServer.host]: CREATE_COMPLETE  state changed
2017-12-08 13:14:06Z [SingleServer]: CREATE_COMPLETE  Stack CREATE completed successfully
+---------------------+-------------------------------------------------+
| Field               | Value                                           |
+---------------------+-------------------------------------------------+
| id                  | 0f5cdf0e-24cc-4292-a0bc-adf2e9f8618a            |
| stack_name          | SingleServer                                    |
| description         | A simple template to deploy your first instance |
| creation_time       | 2017-12-08T13:13:42Z                            |
| updated_time        | None                                            |
| stack_status        | CREATE_COMPLETE                                 |
| stack_status_reason | Stack CREATE completed successfully             |
+---------------------+-------------------------------------------------+
```

Here is a short explanation of the executed command:

The command `openstack stack create` creates the stack, according to the
template defined with `-t SingleServer.yaml`

We set the parameter `key_name` with `--parameter key_name=BEISPIEL` to
fill the `key_name` parameter with BEISPIEL (in this template that installs our BEISPIEL key into your VM). We also named our stack
*SingleServer*.

Finally, we used the `--wait` option to wait and observe the creation
process. If you do not add this option, the command completes
immediately while the creation process continues in the background.

Once the command has completed, you should be able to connect to your VM. First,
we acquire the floating IP of the VM:

```bash
$ openstack stack output show 0f5cdf0e-24cc-4292-a0bc-adf2e9f8618a instance_fip
+--------------+---------------------------------+
| Field        | Value                           |
+--------------+---------------------------------+
| description  | External IP address of instance |
| output_key   | instance_fip                    |
| output_value | 185.116.245.70                  |
+--------------+---------------------------------+
```

Now you can log in to your VM:

```bash
$ ssh ubuntu@185.116.245.70
The authenticity of host '185.116.245.70 (185.116.245.70)' can't be established.
ECDSA key fingerprint is SHA256:kbSkm8eJA0748911RkbWK2/pBVQOjJBASD1oOOXalk.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '185.116.245.70' (ECDSA) to the list of known hosts.
Enter passphrase for key '/Users/ubuntu/.ssh/id_rsa':
```

## Conclusion

Using a heat stack, you combined steps 9 through 11 in a single command.

In the following steps we will look into *Heat* in more detail.
