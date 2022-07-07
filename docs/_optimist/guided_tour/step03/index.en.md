---
title: "03: Spawn a new Stack"
lang: en
permalink: /optimist/guided_tour/step03/
nav_order: 1030
parent: Guided Tour
---

# Step 3: Spawn a new Stack

## Start

In this step you will use the dashboard to spawn a stack that includes a
VM.
You will also get better acquainted with the dashboard.
For this step you need the SSH keypair created in [Step 2](/optimist/guided_tour/step2/).

To spawn a new stack, you need a template that starts a VM.
We recommend using
[SingleServer.yaml](https://github.com/gecio/openstack_examples/blob/master/heat/templates/SingleServer/SingleServer.yaml) from
the [GECio Github Repository](https://github.com/gecio).

Once you have acquired the template, log in to the dashboad with your new
password (see [Step 1](/optimist/guided_tour/step1/)).

Go to *Orchestration → Stacks* and click *LAUNCH STACK*.

![](attachments/13536111.png)

In the following dialog window, for *Template Source* select *File*,
and for *Template File*, use the downloaded `SingleServer.yaml` file. Then click *Next*.

![](attachments/13536112.png)

In the next dialog window, provide the following data:

- Stack Name: BeispielServer
- Creation Timeout: 60
- Password for User: Please use your own password
- availability\_zone: ix1
- flavor\_name: m1.micro
- key\_name: BeispielKey
- machine\_name: singleserver
- public\_network\_id: provider

After you have completed all fields, click *Launch* to spawn the stack.

![](attachments/13536113.png)

The stack will spawn and looks like this.

![](attachments/13536114.png)

You can verify if the stack has correctly started the instance.

Navigate to *Compute* -\> *Instances*. The overview should look like
this:

![](attachments/13536115.png)

You have spawned the stack. Now we will show you how to delete it, including the
VM.

It is also possible to delete the instance itself, but this may cause problems if you want to delete the stack afterwards.

To delete a stack, navigate to *Orchestration* → *Stack*,
and click the *down arrow* symbol behind the Example Stack. Then choose *Delete Stack*.

![](attachments/13536116.png)

## Conclusion

You have created and deleted your first stack.
