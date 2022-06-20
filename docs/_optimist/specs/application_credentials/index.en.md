---
title: Application Credentials
lang: en
permalink: /optimist/specs/application_credentials/
parent: Specifications
nav_order: 9300
---

# Introduction

You can create application credentials to allow your applications to authenticate to the OpenStack Authentication component (Keystone) without using your credentials.

With application credentials, applications can authenticate with the application credential ID and a secret string which is not the user's password. This way, the user password is not embedded in the application’s configuration.

You can delegate a subset of your role assignments on a project to application credentials, granting the application the same or restricted permissions within a project.

## Requirements for Application Credentials

### Name / Secrets

You can generate application credentials for your project with the command line or with the dashboard. These will be associated with the project in which they are created.

The only required parameter to create the credentials is a name, however, you can set a specific secret with the `—-secret` parameter. If the secret parameter is left blank, a secret will be auto-generated in the output instead.

It is important to make a note of the secret in either case as the secret is hashed before it is stored and will not be retrievable after it has been set. If the secret is lost, you must create a new application credential.

### Roles

We also recommend setting the roles that the application credentials should have in the project, since by default, a newly created set of credentials will inherit all available roles.

Below are the available roles that you can assign to a set of application credentials. When you apply these roles to a set of credentials with the `--role` parameter, be aware that all role names are case-sensitive:

- `Member`: The "Member" role only has administrative access to the assigned project.
- `heat_stack_owner`: As "heat_stack_owner" you are able to use and execute existing HEAT templates.
- `load-balancer_member`: As "load-balancer_member" you can use the Octavia LoadBalancer resources.

### Expiration

By default, created application credentials do not expire, however, you can set fixed expiration dates/times for credentials upon creation, using the `--expires` parameter in the command (for example: `--expires '2021-07-15T21:00:00'`).

## Creating Application Credentials with the CLI

You can create a set of application credentials in the desired project with the CLI. The example below shows how to create a set of credentials with the following parameters:

- Name: test-credentials
- Secret: ZYQZm2k6pk
- Roles: Member, heat_stack_owner, load-balancer_member
- Expiration Date/Time: 2021-07-12 at 21:00:00

The new credentials should appear as follows:

```bash
$ openstack application credential create test-credentials --secret ZYQZm2k6pk --role Member --role heat_stack_owner --role load-balancer_member --expires '2021-07-15T21:00:00'
+--------------+----------------------------------------------+
| Field        | Value                                        |
+--------------+----------------------------------------------+
| description  | None                                         |
| expires_at   | 2021-07-15T21:00:00.000000                   |
| id           | 707d14e835124b4f957938bb5a57d1be             |
| name         | test-credentials                             |
| project_id   | c704ac5a32b84b54a0407d28ad448399             |
| roles        | Member heat_stack_owner load-balancer_member |
| secret       | ZYQZm2k6pk                                   |
| system       | None                                         |
| unrestricted | False                                        |
| user_id      | 1d9f1ecb5de3607e8982695f72036fa5             |
+--------------+----------------------------------------------+
```

Note: The secret (whether set by the user or auto-generated) will be displayed upon creation of the credentials. Please take a note of the secret at this time.

## Viewing Application Credentials with the CLI

You can display a list the application credentials belonging to a project with the following command:

```bash
$ openstack application credential list
+----------------------------------+-------------------+----------------------------------+-------------+------------+
| ID                               | Name              | Project ID                       | Description | Expires At |
+----------------------------------+-------------------+----------------------------------+-------------+------------+
| 707d14e835124b4f957938bb5a57d1be | test-credentials  | c704ac5a32b84b54a0407d28ad448399 | None        | None       |
+----------------------------------+-------------------+----------------------------------+-------------+------------+
```

You can view individual credentials with the `$ openstack application credential show <name>` command.

## Deleting Application Credentials via the CLI

You can delete application credentials with the CLI by using the following command with the name or ID of the specific set of credentials:

```bash
openstack application credential delete test-credentials
```

## Creating and Deleting Application Credentials with the Optimist Dashboard

Alternatively, you can also generate application credentials with the Optimist Dashboard under *Identity → Application credentials*:

![](attachments/createappcredentials.png)

Note: You can select multiple roles here by holding the *Shift*-key and navigating through the options.

Once created, a dialog box appears to instruct you to capture the ID and secret. Once done, click *Close*.

![](attachments/secretappcredentials.png)

You can also delete the credentials at any point by using the checkbox to highlight the set of credentials to be deleted and then clicking *DELETE APPLICATION CREDENTIAL*

![](attachments/deleteappcredentials.png)

## Testing Application credentials

Once you have created a set of application credentials either with the CLI or the dashboard, you can test them by using the following curl command to verify that they are working.

You need to use your `<name>` and `<secret>` in the curl command:

```bash
curl -i -H "Content-Type: application/json" -d ' { "auth": { "identity": { "methods": ["application_credential"],  "application_credential": {  "id": “<id>", "secret": “<secret>"}}}}' https://identity.optimist.innovo.cloud/v3/auth/tokens
```

A successful curl attempt outputs an `x-subject-token`, unsuccessful attempts where the credentials are incorrect result in a 401 error.
