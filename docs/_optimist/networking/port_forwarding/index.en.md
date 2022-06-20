---
title: Port Forwarding on Floating IPs
lang: en
permalink: /optimist/networking/port_forwarding/
parent: Networking
nav_order: 2100
---

# Port Forwarding on Floating IPs

Floating IP port forwarding allows users to forward traffic from a TCP/UDP/other protocol port of a floating IP to a TCP/UDP/other protocol port associated to one of the fixed IPs of a Neutron port.

## Create a port forwarding rule on a floating IP

To apply port forwarding on a floating IP, the following information is required:

* The internal IP address to be used
* The UUID of the port to be associated with the floating IP
* The port number of the network port's fixed IPv4 address
* The external port number of the port forwarding’s floating IP address
* The specific protocol to be used in the port forwarding (in this example TCP)
* The floating IP for the port forwarding rule to be applied on

The example below demonstrates the creation of port forwarding on a floating IP, using the required options:

```bash
$ openstack floating ip port forwarding create \
     --internal-ip-address 10.0.0.14 \
     --port 12c29300-0f8a-4c54-a9dc-bee4c12c6ad2 \
     --internal-protocol-port 80 \
     --external-protocol-port 8080 \
     --protocol tcp 185.116.244.141
```

## List port forwarding settings applied to floating IPs

Within a project, you can obtain a list of port forwarding rules applied to specific floating IPs with the following command.

`$ openstack floating ip port forwarding list 185.116.244.141`

You can further refine the command above using `--sort-column` `--port`, `--external-protcol-port` and `--protocol` flags before the floating IP.

## Display details of a specific port forwarding rule

To display the specific details of a port forwarding rule for a floating IP, use the following command:

`$ openstack floating ip port forwarding show <floating-ip> <port-forwarding-id>`

## Modify floating IP port forwarding properties

If a port forwarding configuration on a floating IP has already been created with `$ openstack floating ip port forwarding create`, you can make further changes to the existing configuration with `$ openstack floating ip port forwarding set ...`.

You can modify the following aspects of the port forwarding:

* `--port`: UUID of the network port
* `--internal-ip-address`: Fixed internal address associated with the floating IP port forwarding rule.
* `--internal-protocol-port`: TCP/UDP/etc. port number of the network port fixed IPv4 address associated with the floating IP port forwarding rule
* `--external-protocol-port`: TCP/UDP/etc. port number of the port forwarding rule's floating IP address
* `--protocol`: IP protocol used in the floating IP port forwarding rule (TCP/UDP/other)
* `--description`: Text describing/contextualizing the use of the port forwarding configuration

The configuration of any of the above options can be modified with a variation of the following command:

```bash
$ openstack floating ip port forwarding set \
     --port <port> \
     --internal-ip-address <internal-ip-address> \
     --internal-protocol-port <port-number> \
     --external-protocol-port <port-number> \
     --protocol <protocol> \
     --description <description>] \
    <floating-ip> <port-forwarding-id>`
```

## Delete port forwarding from a floating IP

To remove a port forwarding rule from a floating IP, you need the following information:

* The floating IP from which the port forwarding rule is to be removed from
* The port forwarding ID (this ID is applied upon creation and can be obtained with the `$ openstack floating ip port forwarding list ...` command)

The following command removes the port forwarding rule from a floating IP:

`$ openstack floating ip port forwarding delete <floating-ip> <port-forwarding-id>`
