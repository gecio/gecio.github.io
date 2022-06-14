---
title: "14: Our first steps with HEAT"
lang: en
permalink: /optimist/guided_tour/step14/
nav_order: 1140
parent: Guided Tour
---

# Step 14: Our first steps with HEAT

## Start

In the previous steps, you used a pre-made heat template. Now you will learn how a
heat template works.

## The template

Every heat template follows the following structure:

```yaml
heat_template_version:
 
description:
# The template description
 
parameter_groups:
# Group definitions and their order
 
parameters:
# Parameter definitions
 
resources:
# Resource definitions
 
outputs:
# Definitions of possible outputs
 
conditions:
# Definitions of conditions
```

## Heat Template Version

The template version follows a strict pattern and defines what commands
are available to use.

You can use the following versions, although it is recommended to use the latest
one:

- 2013-05-23
- 2014-10-16
- 2015-04-30
- 2015-10-15
- 2016-04-08
- 2016-10-14
- 2017-02-24

## Description

The description is an optional section that describes the stack. You can
put anything you want here.

We recommend adding a description of how to use the template, what
it is about, and anything else that is useful. This makes
it easier to share with others and reminds you what the
template does in case you have not used it for a while.

You can also add comments by starting them with the *\#* character. It can be used to temporarily disable lines, or to add more documentation to
the template.

## Parameter Groups

In this section, you can specify the parameters, how they should group,
and the order of them.

The groups are divided into a list that contains single parameters.

Every parameter should only have one group because of possible errors
later.
The structure looks like this:

```yaml
parameter_groups:
- label: <name of the group>
  description: <description of the group>
  parameters:
  - <name of the parameter>
  - <name of the parameter>
```

- `label`: Name of the group
- `description`: Description of the group
- `parameter`: A list of all parameters in this group
- `name of the parameter`: Name of the parameter that was defined in the parameter section

## Parameter

In this section you can specify the required parameters for your template.

Parameters are usually used to make it easy to change certain parts of
the template, for example, what SSH key is used.

Each parameter is defined separately, starting with the name, with
the attributes defined underneath:

```yaml
 parameters:
  <Parameter Name>:
    type: <string | number | json | comma_delimited_list | boolean>
    label: <Name of the parameter>
    description: <description of the parameter>
    default: <default of the parameter>
    hidden: <true | false>
    constraints:
      <constraints for the parameter>
    immutable: <true | false>
```

- `Parameter Name`: Name of the parameter
- `type`: The type of the parameter (string, number, json,
    comma\_delimited\_list, boolean)
- `label`: Name of the parameter (optional)
- `description`: The description of the parameter (optional)
- `default`: Default value of the parameter. Will be used, if the
    parameter isn't defined (optional)
- `hidden`:  If the parameter should be hidden in the creation process,
    you can set hidden: *true* as parameter (Optional and set to *false*
    by default)
- `constraints`: You can set a list of constraints. If these aren't
    fulfilled, the stack creation will fail.
- `immutable`: If this parameter is set to true, the parameter can't be
    changed with a stack update. (This will raise an error if attempted)

## Resources

This block specifies the resources that will be created, with every resource in
its own sub block:

```yaml
resources:
  <ID of the resource>:
    type: <resource type>
    properties:
      <name of the property>: <value of the property>
    metadata:
      <specific metatdata>
    depends_on: <Resource ID or a list of resources>
    update_policy: <update rule>
    deletion_policy: <deletion rule>
    external_id: <external resource id>
    condition: <condition name>
```

- `ID of the resource`: Must be unique
- `type`: Type of a resource, for example: OS::NEUTRON::SecurityGroup
    (for a security group) (required)
- `properties`: A list of properties for resources (optional)
- `metadata`: Metadata belonging to the resource  (optional)
- `depends_on`: Resources that the resource depends on (optional)
- `update_policy`: Specifies rules for updates, if required (optional)
- `deletion_policy`: Specifies rules for the deletion. The options are
    *Delete, Retain* and *Snapshot*. With heat\_template\_version 2016-10-14, you can also write them in lowercase.

- `external_id`: You can use external IDs, if required
- `condition`: You can set specific conditions for this resource to be
    created (optional)

## Output

With the output block, you can specify the parameters to be shown after
creation.

Examples could be the IP address of a VM, or the URL of a deployed web
application.

Outputs are specified in sub blocks like this:

```yaml
outputs:
  <name of the output>:
    description: <description>
    value: <value of the output>
    condition: <name of the condition>
```

- `name of the output`: Must be unique
- `description`: If required, you can describe the output (optional)
- `value`: Value of the output (mandatory)
- `condition`: Possible conditions (optional)

## Condition

Like other sections, conditions can also be specified in a block.

You can set conditions, and if they are not fulfilled, the stack creation fails.

```yaml
conditions:
  <name of condition 1>: {term1}
  <name of condition 2>: {term2}
```

- `name of condition`: Must be unique
- `term`: *true* or *false* are expected as a result

## Conclusion

You have learned the basic structure of a heat template and can now start
creating your own.
With this knowledge, you will create your own heat template in the next step.
