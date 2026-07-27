---
title: "mc replicate resync — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-replicate-resync.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-replicate-resync"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc replicate resync — SILO (MinIO Object Storage)"
---
# mc replicate resync

## Syntax

The [`mc replicate resync`](#command-mc.replicate.resync) command resynchronizes all objects in the
specified MinIO bucket to a remote [[administration/bucket-replication#minio-bucket-replication-serverside|replication]] target.

This command requires first configuring the remote bucket target using the
[[reference/minio-mc/mc-replicate-add#command-mc.replicate.add|`mc replicate add`]] command. You must specify the resulting
remote ARN as part of running [`mc replicate resync`](#command-mc.replicate.resync).

This command supports rebuilding a MinIO deployment using an active-active
replication remote as the “backup” source. See the following tutorials
for more information on active-active replication:

[[administration/bucket-replication/enable-server-side-two-way-bucket-replication#minio-bucket-replication-serverside-twoway|Enable Two-Way Server-Side Bucket Replication]]

[[administration/bucket-replication/enable-server-side-multi-site-bucket-replication#minio-bucket-replication-serverside-multi|Enable Multi-Site Server-Side Bucket Replication]]

EXAMPLE

The following command resynchronizes the content of the
`mydata` bucket on the `myminio` MinIO deployment to the remote
MinIO deployment associated to the specified `--remote-bucket`:

```
mc replicate resync start \
   --remote-bucket "arn:minio:replication::d3c086c7-1d64-40c2-954b-fe8222907033:mydata" \
   myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] replicate resync start|status  \
                 --remote-bucket "string"       \
                 [--older-than "string"]        \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.replicate.resync.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment and full path to
the bucket or bucket prefix which MinIO uses as the replication source. For
example, the following command starts replication using the `data`
bucket on the MinIO deployment associated to the `primary` alias.

```
mc replicate resync start primary/data --remote-bucket "ARN"
```

start[](#mc.replicate.resync.start)
Required

Starts the resynchronization procedure using the specified
[`bucket`](#mc.replicate.resync.ALIAS) as the source and the
[`--remote-bucket`](#mc.replicate.resync.-remote-bucket) as the
remote target.

Mutually exclusive with [`mc replicate resync status`](#mc.replicate.resync.status).

status[](#mc.replicate.resync.status)
Required

Returns the status of resynchronization on the specified
[`bucket`](#mc.replicate.resync.ALIAS) to all remote targets.

Include the [`--remote-bucket`](#mc.replicate.resync.-remote-bucket) argument to
filter the status output to only the specified remote target.

--remote-bucket[](#mc.replicate.resync.-remote-bucket)
Required

Specify the ARN for the destination deployment and bucket.

You can retrieve the ARN using [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]] with the `--json` option.
The `rule.Destination.Bucket` field contains the ARN for any given replication rule.

older-than[](#mc.replicate.resync.older-than)
Optional

Specify a duration in days where MinIO only resynchronizes
objects older than the specified duration.

Only valid with [`mc replicate resync start`](#mc.replicate.resync.start).

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Resynchronize Remote Replication Target from Source Bucket

The following [`mc replicate resync`](#command-mc.replicate.resync) command resynchronizes all objects
on the specified source bucket to the remote target regardless of their
replication status:

```
mc replicate resync start --remote-bucket "arn:minio:replication::UUID:data" primary/data
```

Replace `primary/data` with the [[reference/minio-mc/mc-replicate-add#mc.replicate.add.ALIAS|`ALIAS`]] and
full bucket path for which to create the replication configuration.

Replace the [[reference/minio-mc/mc-replicate-add#mc.replicate.add.-remote-bucket|`--remote-bucket`]] value with the
ARN of the remote target. Use [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]] to list
all configured remote replication targets.

## Behavior

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).