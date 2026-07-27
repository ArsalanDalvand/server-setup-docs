---
title: "mc admin bucket remote — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-admin-bucket-remote.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-admin-bucket-remote"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin bucket remote — SILO (MinIO Object Storage)"
---
# mc admin bucket remote

Changed in version RELEASE.2022-12-24T15-21-38Z:

`mc admin bucket remote add` replaced by [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]]

`mc admin bucket remote update` replaced by [[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`mc replicate update`]]

`mc admin bucket remote rm` replaced by [[reference/minio-mc/mc-replicate-rm#command-mc.replicate.rm|`mc replicate rm`]]

`mc admin bucket remote ls` replaced by [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]]

Changed in version RELEASE.2023-02-16T19-20-11Z:

`mc admin bucket remote bandwidth` replaced by [[reference/minio-mc/mc-replicate-status#command-mc.replicate.status|`mc replicate status`]]

Replication related statistics are moving to the `mc replicate status` command.

## Description

The [`mc admin bucket remote`](#command-mc.admin.bucket.remote) command manages the `ARN` resources
for use with [[reference/minio-mc/mc-replicate#command-mc.replicate|`bucket replication`]].

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

## Examples

### Add a New Replication Target

Use [`mc admin bucket remote add`](#mc.admin.bucket.remote.add) to create a new replication target
ARN for use with [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]]:

```
mc admin bucket remote add SOURCE/BUCKET DESTINATION/BUCKET
```

Replace [`SOURCE`](#mc.admin.bucket.remote.add.SOURCE) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment to use as the replication
target. Replace `BUCKET` with the full path of the bucket into which MinIO
replicates objects from the `DESTINATION`.

Replace [`DESTINATION`](#mc.admin.bucket.remote.add.DESTINATION) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment to use as the
replication source. Replace `BUCKET` with the full path of the bucket from
which MinIO replicates objects into the `SOURCE`.

### Remove an Existing Replication Target

Use [`mc admin bucket remote rm`](#mc.admin.bucket.remote.rm) to remove a replication target from a
bucket:

```
mc admin bucket remote rm SOURCE/BUCKET --arn ARN
```

Replace [`SOURCE`](#mc.admin.bucket.remote.rm.SOURCE) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment being used as the
replication source. Replace `BUCKET` with the full path of the bucket from
which MinIO replicates objects.

Replace [`ARN`](#mc.admin.bucket.remote.rm.ARN) with the
ARN of the remote target.

Removing the target halts all in-progress
[[reference/minio-mc/mc-replicate#command-mc.replicate|`bucket replication`]] to the target.

### Retrieve Configured Replication Targets

Use [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]] to list a bucket’s configured replication targets:

```
mc replicate ls ALIAS/PATH
```

Replace [[reference/minio-mc/mc-replicate-ls#mc.replicate.ls.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment being used as the
replication source.
Replace `PATH` with the full path of the bucket from  which MinIO replicates objects.

## Syntax

mc admin bucket remote add[](#mc.admin.bucket.remote.add)

Changed in version RELEASE.2022-12-24T15-21-38Z:

`mc admin bucket remote add` replaced by [[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]]

Adds a remote target to a bucket on a MinIO deployment. The
command has the following syntax:

```
mc admin bucket remote add SOURCE DESTINATION --service "replication" [FLAGS]
```

The command accepts the following arguments:

SOURCE[](#mc.admin.bucket.remote.add.SOURCE)

Required

The full path to the bucket to which the command adds the remote target.
Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment as
the prefix to the bucket path. For example:

```
mc admin bucket remote add play/mybucket
```

DESTINATION[](#mc.admin.bucket.remote.add.DESTINATION)

Required

The target MinIO deployment and bucket.

Specify the full URL to the destination MinIO deployment and bucket
using the following format:

```
http(s)://ACCESSKEY:SECRETKEY@DESTHOSTNAME/DESTBUCKET
```

Replace `ACCESSKEY` with the access key for a user on the

destination MinIO deployment.

Replace `SECRETKEY` with the secret key for a user on the

destination MinIO deployment.

Replace `DESTHOSTNAME` with the hostname and port of the MinIO

deployment (i.e. `minio-server.example.net:9000`).

Replace `DESTBUCKET` with the bucket on the

destination.

--service[](#mc.admin.bucket.remote.add.-service)

Required

Specify `"replication"`.

--region[](#mc.admin.bucket.remote.add.-region)

The region of the [`DESTINATION`](#mc.admin.bucket.remote.add.DESTINATION).

Mutually exclusive with [`add`](#mc.admin.bucket.remote.add)

--path[](#mc.admin.bucket.remote.add.-path)

The bucket path lookup supported by the destination server. Specify
one of the following:

`on`

`off`

`auto` (Default)

Mutually exclusive with
[`add`](#mc.admin.bucket.remote.add)

--sync[](#mc.admin.bucket.remote.add.-sync)

Enables synchronous replication, where MinIO attempts to replicate
the object prior to returning the PUT object response. Synchronous
replication may increase the time spent waiting for PUT operations
to return successfully.

By default, [`mc admin bucket remote add`](#mc.admin.bucket.remote.add) operates in
asynchronous mode, where MinIO attempts replicating objects
after returning the PUT object response.

mc admin bucket remote ls[](#mc.admin.bucket.remote.ls)

Changed in version RELEASE.2022-12-24T15-21-38Z:

`mc admin bucket remote ls` replaced by [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]]

Lists all remote targets associated to a bucket on the MinIO deployment.
Use `mc admin bucket remote ls --help` for usage syntax.

mc admin bucket remote rm, remove[](#mc.admin.bucket.remote.rm)

Changed in version RELEASE.2022-12-24T15-21-38Z:

`mc admin bucket remote rm` replaced by [[reference/minio-mc/mc-replicate-rm#command-mc.replicate.rm|`mc replicate rm`]]

Removes a remote target for a bucket on the MinIO deployment. The
command has the following syntax:

```
mc admin bucket remote rm SOURCE --arn ARN
```

The command accepts the following arguments:

SOURCE[](#mc.admin.bucket.remote.rm.SOURCE)

Required

The full path to the bucket from which the command removes the
remote target. Specify the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment as the
prefix to the bucket path. For example:

```
mc admin bucket remote rm play/mybucket
```

ARN[](#mc.admin.bucket.remote.rm.ARN)

Required

The `ARN` of the remote target for which the command removes from the
target bucket. Use [`mc admin bucket remote ls`](#mc.admin.bucket.remote.ls) to list all remote
targets and their associated ARNs for a specific bucket.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).