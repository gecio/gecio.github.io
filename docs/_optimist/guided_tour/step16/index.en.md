---
title: "16: Let's get to know HEAT better"
lang: en
permalink: /optimist/guided_tour/step16/
nav_order: 1160
parent: Guided Tour
---

Step 16: Let's get to know HEAT better
=======================================

Start
-----

At first glance, it might look like that creating a VM via a heat template and
directly via the OpenStack client take the same time, while this is true if you
only want to create the VM once, this is true but the real advantage to heat is
in reusing templates.

Now that we have our simple template, we'll get to know heat a bit better by
adding a variable parameter to our template.

Parameter
---------

In this example, we'll add a parameter for the SSH key. The advantage of this
is that we can use a VM with different keys without changing our template.

We need to define the parameter and also define its type, the proper type for
what we want to accomplish is `string`:

```yaml
heat_template_version: 2014-10-16
 
parameters:
    key_name:
        type: string
```

Now that we've defined our first parameter, we'll add the same resource to
our template like this:

```yaml
heat_template_version: 2014-10-16

parameters:
    key_name:
        type: string


resources:
    Instanz:
    type: OS::Nova::Server
    properties:
        key_name: Beispiel
        image: Ubuntu 16.04 Xenial Xerus - Latest
        flavor: m1.small
```

Now we'll actually use our parameter, we'll replace *Beispiel* with
our parameter.

This is done with the get\_param syntax (for getting the parameter).

The template is now ready to use and we can define the key\_name from the
command line like in our previous command line:

```yaml
heat_template_version: 2014-10-16

parameters:
    key_name:
        type: string

resources:
    Instanz:
    type: OS::Nova::Server
    properties:
        key_name: { get_param: key_name }
        image: Ubuntu 16.04 Xenial Xerus - Latest
        flavor: m1.small
```

Conclusion
------------

We've now added a variable parameter to our template! In the next step, we will add the network.
