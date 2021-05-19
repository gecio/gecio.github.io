---
title: "15: The first heat template"
lang: en
permalink: /optimist/guided_tour/step15/
nav_order: 1150
parent: Guided Tour
---

Step 15: The first heat template
================================

Start
-----

In the previous step, we've learnt the basic layout of a heat template, now
we're going to put that knowledge to use.

The first template
------------------

As we've mentioned earlier, our templates needs to start with a version
definition.

In this example, we'll use *2016-10-14* as our version. We mentioned other
versions in the previous step.

Our template now contains this:

```yaml
heat_template_version: 2016-10-14
```

Even though it's optional, it's best practice to add a description to our
templates.

```yaml
heat_template_version: 2016-10-14
 
description: A simple template to deploy a vm
```

Next up, we're going to add the resource "Instanz".

Be sure to pay close attention to the structure of our template and to
indent the "*Instanz*" under *resources*.

To indent, use 4 spaces and take care not to use tab, if you use tabs or an
inconsistent amount of spaces, it will cause errors that are hard to find.

The state of our template should look like this:

```yaml
heat_template_version: 2016-10-14

description: A simple template to deploy a vm

resources:
    Instanz:
```

Next, we'll define the type of the resource.

A detailed list of all types can be found in the [official OpenStack
documentation](https://docs.openstack.org/developer/heat/template_guide/openstack.html)

In our example, we'll define *Instanz* as a VM:

```yaml
heat_template_version: 2016-10-14
 
description: A simple template to deploy a vm

resources:
    Instanz:
    type: OS::Nova::Server
```

Now that we've defined the type, we'll define its properties.

Let's define the key, image and the flavor:

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

Conclusion
-----------

We've now defined a template that creates a single VM instance, if you want,
you could run it like we did previously in [Step 13: "The structured way to create an instance (with stacks)"](/optimist/guided_tour/step13/).
