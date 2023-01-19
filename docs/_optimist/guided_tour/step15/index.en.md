---
title: "15: The first heat template"
lang: "en"
permalink: /optimist/guided_tour/step15/
nav_order: 1150
parent: Guided Tour
---

# Step 15: The first heat template

## Start

In the previous step, you learned the basic layout of a heat template. Now you can create your own.

## The first template

As stated earlier, your template needs to start with a version definition.

Version *2016-10-14* is used for the below example:

```yaml
heat_template_version: 2016-10-14
```

Although it is optional, it is best practice to add a description to your template.

```yaml
heat_template_version: 2016-10-14
 
description: A simple template to deploy a vm
```

Next, add the resource "Instanz".

Be sure to pay attention to the structure of our template and to
indent the "*Instanz*" under *resources*.

To indent, use 4 spaces (not tabs). If you use tabs or an
inconsistent amount of spaces, it will cause errors that may be hard to locate.

Your template should now look like this:

```yaml
heat_template_version: 2016-10-14

description: A simple template to deploy a vm

resources:
    Instanz:
```

Next, define the resource type.

A detailed list of all types is available in the [official OpenStack
documentation](https://docs.openstack.org/developer/heat/template_guide/openstack.html)

In our example, you can define *Instanz* as a VM:

```yaml
heat_template_version: 2016-10-14

description: A simple template to deploy a vm

resources:
    Instanz:
    type: OS::Nova::Server
```

Now that you have defined the type, you should next define its properties.

Let's define the key, image, and the flavor:

```yaml
heat_template_version: 2016-10-14

description: A simple template to deploy a vm

resources:
    Instanz:
    type: OS::Nova::Server
    properties:
        key_name: BeispielKey
        image: Ubuntu 16.04 Xenial Xerus - Latest
        flavor: m1.small
```

## Conclusion

You have now defined a template that creates a single VM instance. If you like, you can run it like you did previously in [Step 13: "The structured way to create an instance (with stacks)"](/optimist/guided_tour/step13/).
