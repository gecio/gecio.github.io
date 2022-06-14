---
title: "15: The first heat template"
lang: en
permalink: /optimist/guided_tour/step15/
nav_order: 1150
parent: Guided Tour
---

# Step 15: The first heat template

## Start

In the previous step, you learnt the basic layout of a heat template. Now
you are going to put that knowledge to use.

## The first template

As we have mentioned earlier, our template needs to start with a version
definition.

In this example, we will use *2016-10-14* as our version. We mentioned other
versions in the previous step.

Our template now contains this:

```yaml
heat_template_version: 2016-10-14
```

Even though it is optional, it is best practice to add a description to our
template.

```yaml
heat_template_version: 2016-10-14
 
description: A simple template to deploy a vm
```

Next, you add the resource "Instanz".

Be sure to pay attention to the structure of our template and to
indent the "*Instanz*" under *resources*.

To indent, use 4 spaces and make sure not to use tab. If you use tabs or an
inconsistent amount of spaces, it will cause errors that are hard to find.

The state of your template should look like this:

```yaml
heat_template_version: 2016-10-14

description: A simple template to deploy a vm

resources:
    Instanz:
```

Next, you define the type of the resource.

A detailed list of all types is available in the [official OpenStack
documentation](https://docs.openstack.org/developer/heat/template_guide/openstack.html)

In our example, we define *Instanz* as a VM:

```yaml
heat_template_version: 2016-10-14

description: A simple template to deploy a vm

resources:
    Instanz:
    type: OS::Nova::Server
```

Now that you have defined the type, you will define its properties.

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

You have now defined a template that creates a single VM instance. If you like,
you can run it like you did previously in [Step 13: "The structured way to create an instance (with stacks)"](/optimist/guided_tour/step13/).
