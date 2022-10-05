---
title: Secure backups with restic and rclone
lang: en
permalink: /optimist/storage/s3_documentation/backupwithrestic/
nav_order: 3170
parent: S3 Compatible Object Storage
grand_parent: Storage
---

Restic is a very simple and powerful file-level backup solution, which is rapidly gaining popularity. It can be used in combination with S3 which makes it a great tool to use with Optimist.

## Problem statement

When performing file-level backups of a node into S3, the backup software needs write permissions for the S3 bucket.

But if attackers gain access to the machine, they can also destroy the backups in the bucket, since the S3 credentials are present on the compromised system.

The solution can be as simple as limiting the level of access of the backup software to the bucket. Unfortunately, this is not trivial with S3.

## Background

[S3 access control lists (ACLs)](/optimist/storage/s3_documentation/security) enable you to manage access to buckets and objects, but they have limitations. They essentially differentiate READ and WRITE permissions:

* READ - Allows grantee to list the objects in the bucket
* WRITE - Allows grantee to create, overwrite, and delete any object in the bucket

The limitations of ACLs were addressed by the access policy permissions (ACP). We can attach a no-delete policy to the bucket, for example:

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "nodelete1",
            "Effect": "Deny",
            "Action": [
                "s3:DeleteBucket",
                "s3:DeleteBucketPolicy",
                "s3:DeleteBucketWebsite",
                "s3:DeleteObject",
                "s3:DeleteObjectVersion"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ]
        }
    ]
}
```

Unfortunately, the S3 protocol itself was not designed with the concept of WORM (write once read many) backups in mind. Access policy permissions do not differentiate between changing an existing object (which would effectively allow deleting it) and creating a new object.

Attaching the above policy on a bucket does not prevent the objects in it from being overwritten.

```bash
$ s3cmd put testfile s3://appendonly-bucket/testfile
upload: 'testfile' -> 's3://appendonly-bucket/testfile' [1 of 1]
 1054 of 1054 100% in 0s 5.46 KB/s done     # policy allows write
$ s3cmd rm s3://appendonly-bucket/testfile
ERROR: S3 error: 403 (AccessDenied)         # policy denies deletion
$
$ s3cmd put testfile s3://appendonly-bucket/testfile
upload: 'testfile' -> 's3://appendonly-bucket/testfile' [1 of 1]
 1054 of 1054 100% in 0s 5.50 KB/s done     # :(
```

## Proposed solution

Since an attacker on a compromised system will always have access to the S3 credentials and every service running on the system - including restic itself, proxies, etc. - we need a second, locked-down VM which can restrict delete operations. Restic can be integrated perfectly with rclone, so we will use it in this example.

### Our Environment

* `appsrv`: the system which has access to the files we would like to backup
* `rclonesrv`: the system running the rclone proxy (and nothing else, to minimise the attack surface)

### Set up the rclone proxy

1. Install rclone on the `rclonesrv`:

    ```bash
    sudo apt install rclone
    ```

1. Create user for rclone:

    ```bash
    sudo useradd -m rcloneproxy
    sudo su - rcloneproxy
    ```

1. Create rclone backend configuration:

    ```bash
    mkdir -p .config/rclone
    cat << EOF > .config/rclone/rclone.conf
    [s3-resticrepo]
    type = s3
    provider = Other
    env_auth = false
    access_key_id = 111122223333444455556666
    secret_access_key = aaaabbbbccccddddeeeeffffgggghhhh
    region = eu-central-1
    endpoint = s3.es1.fra.optimist.innovo.cloud
    acl = private
    bucket_acl = private
    upload_concurrency = 8
    EOF
    ```

1. Verify that the access to the repository is working with:

    ```bash
    rclone lsd s3-resticrepo:databucket
            0 2021-11-21 20:02:10        -1 data
            0 2021-11-21 20:02:10        -1 index
            0 2021-11-21 20:02:10        -1 keys
            0 2021-11-21 20:02:10        -1 snapshots
    ```

### Configure the appserver

1. Generate an SSH-Keypair on `appsrv` with the user you are performing the backup with:

    ```bash
    ssh-keygen -o -a 256 -t ed25519 -C "$(hostname)-$(date +'%d-%m-%Y')"
    ```

1. Set the environment variables for restic:

    ```bash
    export RESTIC_PASSWORD="MyV3ryS3cUr3r3571cP4ssW0rd"
    export RESTIC_REPOSITORY=rclone:s3-resticrepo:databucket
    ```

### Restrict the SSH key to restic-only commands

The last step is to go back to the `rclonesrv` and edit the SSH `authorized_keys` file to restrict the newly generated SSH key to a single command. This way, an attacker is not able to use the ssh keypair to run arbitrary commands on the rclone proxy and compromise the backups.

```bash
vi ~/.ssh/authorized_keys
# add an entry with the restic user's public key generated in a previous step:
command="rclone serve restic --stdio --append-only s3-resticrepo:databucket" ssh-ed25519 AAAAC3fdsC1lZddsDNTE5ADsaDgfTwNtWmwiocdT9q4hxcss6tGDfgGTdiNN0z7zN appsrv-18-11-2021
```

## Using restic with the rclone proxy

With the environment variables set, restic should work now from `appsrv`.

Example backing up `/srv/myapp`:

```bash
restic -o rclone.program="ssh rcloneproxy@rclonesrv.mydomain.com" backup /srv/myapp
```

Listing snapshots:

```bash
restic -o rclone.program="ssh rcloneproxy@rclonesrv.mydomain.com" snapshots
```

Deleting snapshots:

```bash
restic -o rclone.program="ssh rcloneproxy@rclonesrv.mydomain.com" forget 2738e969
repository b71c391e opened successfully, password is correct
Remove(<snapshot/2738e9693b>) returned error, retrying after 446.577749ms: blob not removed, server response: 403 Forbidden (403)
```

Right, that does not work. That was our goal.

## Summary

This way:

* The rclone proxy does not even run on the `rclonesrv` as a service. It will just be spawned on demand, for the duration of the restic operation. Communication happens over HTTP2 over stdin/stdout, in an encrypted SSH tunnel.
* Since rclone is running with `--append-only`, it is not possible to delete (or overwrite) snapshots in the S3 bucket.
* All data (except credentials) is encrypted/decrypted locally, then sent/received via `rclonesrv` to/from S3.
* All the credentials are **only** stored on `rclonesrv` to communicate with S3.

Since the command is hard-coded into the SSH configuration for the user's SSH key, there is no way to use the keys to get access to the rclone proxy.

## Some more thoughts

The advantages of this construct are probably clear already by now. Furthermore:

* Managing snapshots (both manually and with a retention policy) is only possible on the rclone proxy.
* A single rclone proxy VM (or even a docker container on an isolated VM) can serve multiple backup clients.
* It is highly recommended to use one key for every server which backs up data.
* If you like to use more than one repository out of a node, you need new SSH keys for them. You can then specify which key to use with `-i ~/.ssh/id_ed25519_another_repo` in the `rclone.program` arguments just like you would with SSH.
