---
title: "06: Create and use our own SSH-Key"
lang: en
permalink: /optimist/guided_tour/step06/
nav_order: 1060
parent: Guided Tour
---

# Step 6: Create and use our own SSH-Key

## Start

To access your VMs with SSH you need to create an SSH keypair.

If you already have a keypair, you do not need to create a new one. The
only exception to this is if you have an ED25519
keypair. This keypair is not usable due to a bug in the OpenStack OpenSSL.

## Creation

As mentioned in step 2, there are many ways to create an SSH keypair.

Here you will create one from the console using the following command:

```text
$ ssh-keygen -t rsa -f Beispiel.key
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in Beispiel.key.
Your public key has been saved in Beispiel.key.pub.
The key fingerprint is:
SHA256:UKSodmr6MFCO1fSqNYAoyM7uX8n/O5a43cPEV5vJXW8
The key's randomart image is:
+---[RSA 2048]----+
|    .  .o        |
|+. o o o         |
|=.+ o +          |
|+= o . .       ..|
|oo+ =   S .   o B|
|o. =...    o . =E|
|o.+  +  . + .  . |
|.=  . ...+.o     |
|.oo.   o++o..    |
+----[SHA256]-----+
```

The command above generates two files, this is why it is referred to as
keypair.

The two generated files are Beispiel.key (private key) and
Beispiel,key.pub (public key).

**You should always keep your private key at a secure location, while you distribute the public key to places where you want access to.**

## Installation

To start using your new keypair, you need to add it to your OpenStack environment. You will do this with the OpenStack client.

Use the command below (in our example, the created keypair is stored in
`~/.ssh/`. If your keys are saved in a different location, you need to copy the
keypair to `~/.ssh/`).

```bash
$ openstack keypair create --public-key ~/.ssh/Beispiel.key.pub Beispiel
+-------------+-------------------------------------------------+
| Field       | Value                                           |
+-------------+-------------------------------------------------+
| fingerprint | ec:a6:75:f9:33:4b:e0:ba:e7:bb:b6:8a:a1:5d:48:ff |
| name        | Beispiel                                        |
| user_id     | 9bf501f4c3d14b7eb0f1443efe80f656                |
+-------------+-------------------------------------------------+
```

You can check if everything worked by listing the keys. The one you
just uploaded should be also visible.

```bash
$ openstack keypair list
+----------+-------------------------------------------------+
| Name     | Fingerprint                                     |
+----------+-------------------------------------------------+
| Beispiel | ec:a6:75:f9:33:4b:e0:ba:e7:bb:b6:8a:a1:5d:48:ff |
+----------+-------------------------------------------------+
```

## Conclusion

You have now generated a keypair and uploaded the public key. You can
use it to log in to your new Instances.

We will explain this in step 7.
