---
title: "mc replicate rm — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-replicate-rm.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-replicate-rm"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc replicate rm — SILO (MinIO Object Storage)"
---
# mc replicate rm

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc replicate rm` replaces the `mc admin bucket remote rm` command.
Removing the replication automatically removes the underlying remote target.

## Syntax

The [`mc replicate rm`](#command-mc.replicate.rm) command removes a
[[administration/bucket-replication#minio-bucket-replication-serverside|replication rule]] from a
MinIO bucket.

The [`mc replicate remove`](#command-mc.replicate.remove) command has equivalent functionality to [`mc replicate rm`](#command-mc.replicate.rm).

```
mc [GLOBALFLAGS] replicate rm FLAGS [FLAGS] ALIAS
```

EXAMPLE

The following command removes the replication rule with specified
id from the `mydata` bucket on the `myminio` MinIO deployment:

```
mc replicate rm --id "c76um9h4b0t1ijr36mug" myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] replicate rm     \
                 --id "string"    \
                 [--all --force]  \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.replicate.rm.ALIAS)

Required the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment and full path to
the bucket or bucket prefix from which to remove the replication rule. For
example:

```
mc replicate rm --id "ID" myminio/mybucket
```

--id[](#mc.replicate.rm.-id)

Required Specify the unique ID for a configured replication rule.

You can omit this option if specifying [`--all`](#mc.replicate.rm.-all)

--all[](#mc.replicate.rm.-all)

Optional Removes all replication rules on the specified bucket. Requires
specifying the [`--force`](#mc.replicate.rm.-force) flag.

--force[](#mc.replicate.rm.-force)

Optional Required if specifying [`--all`](#mc.replicate.rm.-all) .

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Remove a Replication Rule from a Bucket

Use [`mc replicate rm`](#command-mc.replicate.rm) to remote a bucket replication rule:

```
mc replicate rm --id "ID" ALIAS/PATH
```

Replace [`ID`](#mc.replicate.rm.-id) with the unique ID of the
replication rule to remove. Use [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]] to list all
replication rules for the bucket.

Replace [`ALIAS`](#mc.replicate.rm.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`PATH`](#mc.replicate.rm.ALIAS) with the path to the
bucket or bucket prefix.

### Remove All Replication Rules from a Bucket

Use [`mc replicate rm`](#command-mc.replicate.rm) to list bucket replication rules:

```
mc replicate rm --all --force ALIAS/PATH
```

Replace [`ALIAS`](#mc.replicate.rm.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`PATH`](#mc.replicate.rm.ALIAS) with the path to the
bucket or bucket prefix.

## Behavior

### Removing Replication Rules Does Not Affect Replicated Objects

Removing one or all replication rule for a bucket does not
remove any objects already replicated under those rule(s).

Use The command or [[reference/minio-mc/mc-rb#command-mc.rb|`mc rb`]] commands to remove replicated
objects on the remote target. You can identify replicated objects using
the `X-Amz-Replication-Status` metadata field where the value is
`REPLICA`. Buckets which contain objects from multiple replication sources
may require additional care and filtering to determine the source prior
to removal.

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