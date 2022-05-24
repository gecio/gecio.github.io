---
title: "23: Object Storage (S3 compatible)"
lang: en
permalink: /optimist/guided_tour/step23/
nav_order: 1230
parent: Guided Tour
---

Step 23: Object Storage (S3 compatible)
=================================================

Preface
-------

In our previous steps, we learned about various building blocks in OpenStack.
Now we will take a look at the [Object Storage](https://en.wikipedia.org/wiki/Object_storage), which gives us some interesting possibilities to save data.

Credentials
-----

We will need another set of credentials to get access to the Object Storage.
Therefore, we will need the OpenStackClient (as mentioned in [Step 4](/optimist/guided_tour/step04/)), to create the credentials via the OpenStack API.
Run the following command to create the credentials::

```bash
openstack ec2 credentials create
```

If everything works fine, it should look like this:

```bash
$ openstack ec2 credentials create
+------------+-----------------------------------------------------------------+
| Field      | Value                                                           |
+------------+-----------------------------------------------------------------+
| access     | <your access_key>                                               |
| links      | {u'self': u'https://identity.optimist.innovo.cloud/v3/users/    |
|            | user-id/credentials/OS-EC2/access_key'}                         |
| project_id | <your project_id>                                               |
| secret     | <your secret_key>                                               |
| trust_id   | None                                                            |
| user_id    | <your user_id>                                                  |
+------------+-----------------------------------------------------------------+
```

Once the credentials are created we need some tooling to interact with the ObjectStorage.

How to get access to the ObjectStorage (S3 compatible)
---------

There are several tools available to interact with the ObjectStore, however we recommend [s3cmd](https://s3tools.org/s3cmd) as it is easy to control and use.

We’ve already installed "pip" as package-manager (in Step 4) and can use it to install  [s3cmd](https://s3tools.org/s3cmd):

```bash
pip install s3cmd
```

After the installation it’s necessary to enter the credentials in the *.s3cfg* .
It's located in the user's home directory, e.g. /home/username/
If the .s3cfg isn’t already there, please create it.

```bash
touch .s3cfg
```

Now you can open the .s3cfg with your preffered text editor (i.e. vi, vim, nano) and input your credentials:

```bash
access_key = <your access_key>
check_ssl_certificate = True
check_ssl_hostname = True
host_base = s3.es1.fra.optimist.innovo.cloud
host_bucket = s3.es1.fra.optimist.innovo.cloud
secret_key = <your secret_key>
use_https = True
```

The bucket
---------

After we have access to the ObjectStorage (S3 compatible), let’s start to work with it.
If needed, you can see all s3cmd commands with:

```bash
s3cmd --help
```

Now we will create a bucket.
Buckets are similar to folders, which we need for a structure.
A file can only be saved in a bucket and it is also important, that the name is unique (for all customers)
If there is already a bucket with the name „test, there is no possibility to create another one with the name „test“
We recommend a UUID for a bucket.

Also, there is the possibility to differentiate between public and private buckets.
All buckets are by default private and can only be accessed by the creator of the bucket.
If needed, it can be changed as an example via the *Access Control List (ACL)*.
IMPORTANT: If you set a bucket to public, all files in it are reachable. We recommend to set only files public.

After some basics, it’s time to create a bucket with a UUID:

```bash
$ s3cmd mb s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189
Bucket 's3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/' created
```

Upload a file
---------

After the creation of a bucket, let's upload a file.
Therefore, we use the command `s3cmd put file_name s3://bucket_name` and a possible outcome can look like this:

```bash
$ s3cmd put test.yaml s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189
upload: 'test.yaml' -> 's3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml'  [1 of 1]
 4218 of 4218   100% in    0s     4.61 kB/s  done
```

Get access to the files
---------

As we already mentioned there is the possibility to reach the files. The general URL for this is *<https://s3.es1.fra.optimist.innovo.cloud/bucket_name/file_name>*

To get access to our example file, we need to change the settings from *private* to *public*.
We will use the *Access Control List (ACL)* for this:

```bash
$ s3cmd setacl s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml --acl-public
s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml: ACL set to Public  [1 of 1]
```

Now it's possible to download the file and the link for it is:
<https://s3.es1.fra.optimist.innovo.cloud/e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml>

If you want to set the file again *private*, use this command:

```bash
$ s3cmd setacl s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml --acl-private
s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml: ACL set to Private  [1 of 1]
```

Recursively Deleting Containers Using the OpenStack CLI
----

Attempting to delete a container which has remaining objects is not possible via the OpenStack CLI. We therefore need to clean up the contents and the container recursively with the following command:

```bash
openstack container delete -r example-container
```

To cleanup and delete containers with many objects (100+), we recommend the following approach to avoid timeouts when using the recursive delete command:

```bash
while true; do time openstack container delete -r example-container; sleep 2; done
```

Conclusion
---------

We made our first steps in an S3 compatible Storage and also learned some basics to use it.
