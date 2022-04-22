---
title: S3 Security
lang: en
permalink: /optimist/storage/s3_documentation/security/
nav_order: 3150
parent: S3 Compatible Object Storage
grand_parent: Storage
---

# S3 Security

## Introduction

This page gives an overview of the following topics relating to S3 buckets / Swift:

* Container Access Control Lists (ACLs)
* Bucket Policies

Operations on container ACLs must be performed at the OpenStack level using Swift commands, while Bucket Policies must be set on each bucket within a project using the s3cmd command line. In this document we will outline some examples of each type of operation.

## Container Access Control Lists (ACLs)

By default, only project owners have permission to create, read and modify containers and objects. However, an owner can grant access to other users on by using an Access Control List (ACL). The ACL can be set on each container, and is applicable only to that container and the objects within that container.

Some of the main elements which can be used to set an ACL on a container are listed below:

| **Element**          | **Description**                                       |
|:---------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `.r:*.`              | Any user has access to objects. No token is required in the request.                          |
| `.r:<referrer>`      | The referrer is granted access to objects. The referrer is identified by the Referer request header in the request. No token is required.        |
| `.r:-<referrer>`     | This syntax (with “-” prepended to the referrer) is supported. However, it does not deny access if another element (e.g., .r:*) grants access.       |
| `.rlistings`         | Any user can perform HEAD or GET operations on the container if the user also has read access on objects (e.g., also has `.r:*` or `.r:<referrer>.` No token is required. |

As an example, we will set the policy `.r:*.` on a container called `<example-container>`. This policy will allow any external user access to the objects within the container.

```bash
swift post example-container --read-acl ".r:*"
```

Conversely, we can also allow users to list but not access the list of objects within a container by setting the `.rlistings` policy on our `example-container`:

```bash
swift post example-container --read-acl ".rlistings"
```

To remove any read policy and set the container to its default private state, the following command can be used:

```bash
swift post -r "" example-container
```

To check which ACL is set on a container, use the following command.

```bash
swift stat example-container
```

This gives an overview of the stats for the container and displays the current ACL rule for a container.

### Prevent Listing on Containers when using the `.r:*.` policy:

In the current version of OpenStack, to prevent the contents from being listed while using the  `.r:*.`  policy on a container, we recommend creating an empty index.html object within the container. This will allow users to download objects without listing the contents of the buckets. This can be accomplished with the following steps:

Firstly, add the blank `index.html` file to our `example-container`:

```bash
swift post -m 'web-index: index.html’ example-container
```

Then create the index.html file as an object within the container:

```bash
touch index.html && openstack object create example-container index.html
```

This will allow external users access to specific files without listing the contents of the container.

## Bucket Policies

Bucket policies are used to control access to each bucket in a project. It is recommended to set a policy on all buckets upon creation.

The first step is to create a policy as follows. The following template only requires that you change the bucket name for subsequent policies, The below example creates a policy for bucket `example-bucket`:

```bash
cat > examplepolicy
{
    "Version": "2008-10-17",
    "Statement": [
        {
        "Sid": "AddPerm",
        "Effect": "Allow",
        "Principal": "*",
        "Action": "s3:GetObject",
        "Resource": "arn:aws:s3:::example-bucket/*"
        }
    ]
}
```

Breakdown of each element within the above policy example:

* `Version`: Specifies the language syntax rules to be used to process the policy. It is recommended to always use: “2012-10-17” as this is the current version of the policy language.
* `Statement`: The main element of the policy, the other elements are located within this statement.
* `SID`: The Statement ID, this is an optional identifier which can be used to describe the policy statement. Recommended so that the purpose of each policy is clear.
* `Effect`: Set either to “Allow” or “Deny"
* `Principal`: Specifies the principal that is allowed or denied access to a resource. Here, the wildcard "*” is used to apply the rule to all.
* `Action`: Describes the specific actions that will be allowed or denied.

(For further information on the available policy options and how to tailor this to your specific needs, please see the [official AWS documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html)).

Next, apply the newly created policy to bucket `example-bucket`:

```bash
s3cmd setpolicy examplepolicy s3://example-bucket
```

You will also be able to run the following command afterwards to see that the policy is in place:

```bash
s3cmd info s3://example-bucket
```

Once the policy is applied, you can set once again set `Public Access: Disabled` on the Dashboard.

Once the above steps have been taken we will have the following results:

* The container will be private, and files will not be listed or displayed via XML.
* The policy now allows access to specific files with a direct link.
