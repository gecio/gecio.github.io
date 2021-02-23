---
title: Step 6 - Create and use our own SSH-Key
lang: en
permalink: /optimist/guided_tour/step06
nav_order: 2600
parent: Guided Tour
---

Step 6: Create and use our own SSH-Key
======================================

Start
-----

In order to access our VMs via SSH we need to create an SSH keypair.

If you already have a keypair, we don't need to create a new one. The
only exception to this is if we the keypair we have is an ED25519
keypair, these are not usable because of a bug in OpenStack's
OpenSSL.

Creation
--------

As mentioned in step 2, there are many ways to create an SSH keypair.

In this step we will create one from the console with this command:

```
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

The command above will generate two files, this is why we refer to it as
a keypair.

The two files generated are Beispiel.key (the private key) and
Beispiel,key.pub (The public key)

**You should keep your private key to yourself, while we will distribute the public key to places where we want access to.**

Installation
------------

To start using our new keypair, we need to add it to our OpenStack environment,
which we'll do with the OpenStack client.

We will use the command below (in our example, the created keypair is stored in
`~/.ssh/`, if your keys are saved in a different location, you need to copy the
keypair to `~/.ssh/`)

```
$ openstack keypair create --public-key ~/.ssh/Beispiel.key.pub Beispiel
+-------------+-------------------------------------------------+
| Field       | Value                                           |
+-------------+-------------------------------------------------+
| fingerprint | ec:a6:75:f9:33:4b:e0:ba:e7:bb:b6:8a:a1:5d:48:ff |
| name        | Beispiel                                        |
| user_id     | 9bf501f4c3d14b7eb0f1443efe80f656                |
+-------------+-------------------------------------------------+
```

We can check if everything worked by listing the keys and seeing the one we
just uploaded:

```
$ openstack keypair list
+----------+-------------------------------------------------+
| Name     | Fingerprint                                     |
+----------+-------------------------------------------------+
| Beispiel | ec:a6:75:f9:33:4b:e0:ba:e7:bb:b6:8a:a1:5d:48:ff |
+----------+-------------------------------------------------+
```

Conclusion
----------

Now that we have a keypair generated and uploaded the public key, we can
use it to log in to our new VMs!
