---
title: "23: Object Storage (S3 compatible)"
lang: "en"
permalink: /optimist/guided_tour/step23/
nav_order: 1230
parent: Guided Tour
---

# Step 23: Object Storage (S3 compatible)

## Start

In the previous steps, you learned about various building blocks in OpenStack.
Now we will take a look at the [Object Storage](https://en.wikipedia.org/wiki/Object_storage), which offers some interesting ways to save data.

## Credentials

The first step is to obtain login data (ec2 credentials) in order to access Object Storage.
Therefore, you need the OpenStackClient (as mentioned in [Step 4: Our way to the console"](/optimist/guided_tour/step04/)), to create the credentials with the OpenStack API.
To create the credentials, run the following command:

```bash
openstack ec2 credentials create
```

The output should look like this:

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

Once the credentials have been created, you need some tools to interact with the ObjectStorage.

## How to get access to the ObjectStorage (S3 compatible)

There are several tools available which allow us to interact with Object Storage, however we recommend using [s3cmd](https://s3tools.org/s3cmd) as it is straightforward to use and handle.

You have already installed "pip" as package-manager (in [Step 4](/optimist/guided_tour/step04/)) you can also use it to install [s3cmd](https://s3tools.org/s3cmd):

```bash
pip install s3cmd
```

Since S3cmd is now installed, the previously created credentials must be entered in a file called *.s3cfg* in order to begin using it. The file should be located in the user's home directory, for example, `/home/username/`

The following process can now be used to create the *.s3cfg* file:

```bash
touch .s3cfg
```

You can open *.s3cfg* with your preferred text editor (for example, *vi, vim, nano*) and enter your credentials as follows:

```bash
access_key = <your access_key>
check_ssl_certificate = True
check_ssl_hostname = True
host_base = s3.es1.fra.optimist.innovo.cloud
host_bucket = s3.es1.fra.optimist.innovo.cloud
secret_key = <your secret_key>
use_https = True
```

## The bucket

After you have access to *ObjectStorage* (S3 compatible), you can start working with it.
If required, you can see all s3cmd commands with:

```bash
s3cmd --help
```

You can now create a bucket. In the broadest sense, buckets are similar to folders, which are required for a structure.
A file can only be saved in a bucket. It is important that the name is unique (for all customers).
If there is already a bucket available with the name *test*, you cannot create another one with the name *test*.
We recommend using a UUID and then resolving it in the corresponding application.

You can also differentiate between *public* and *private* buckets.
By default, all buckets are private, and only the creator of the bucket can access them.
If needed, you can change it, for example with the *Access Control List (ACL)*.
IMPORTANT: If you set a bucket to public, all files in it are reachable. Information about files in this bucket that are set to private can also be retrieved. We recommend only setting specific files to public.

Now that we know the key details, it is time to create a bucket with a UUID:

```bash
$ s3cmd mb s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189
Bucket 's3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/' created
```

## Upload a file

After the bucket has been created, let's upload a file with the command `s3cmd put file_name s3://bucket_name`. The outcome should be similar to the below:

```bash
$ s3cmd put test.yaml s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189
upload: 'test.yaml' -> 's3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml'  [1 of 1]
 4218 of 4218   100% in    0s     4.61 kB/s  done
```

## Get access to the files

The general URL for accessing files in Optimist is *<https://s3.es1.fra.optimist.innovo.cloud/bucket_name/file_name>*.

To get access to your example file, you need to change the settings from *private* to *public*.
To do this, use the *Access Control List (ACL)*:

```bash
$ s3cmd setacl s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml --acl-public
s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml: ACL set to Public  [1 of 1]
```

Now you can access the file with the following link:
<https://s3.es1.fra.optimist.innovo.cloud/e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml>

To set the file to *private* once again, use this command:

```bash
$ s3cmd setacl s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml --acl-private
s3://e4d05df3-aa8e-4a37-b1b5-2745d189b189/test.yaml: ACL set to Private  [1 of 1]
```

## Conclusion

You have taken your first steps with S3 compatible storage.
