---
title: "mc support top locks — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-top-locks.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-top-locks"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support top locks — SILO (MinIO Object Storage)"
---
# mc support top locks

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Syntax

The [`mc support top locks`](#command-mc.support.top.locks) command lists the ten oldest [[administration/object-management/object-retention#minio-object-locking|locks]] on a MinIO deployment.

The command outputs the age of the lock, type of lock, owner, and resource.
The output resembles the following:

```
Since                 Type    Owner                 Resource
13 hours ago          WRITE   10.68.100.18:9000     .minio.sys/leader.lock
13 hours ago          WRITE   10.68.100.18:9000     .minio.sys/callhome/runCallhome.lock
13 hours ago          WRITE   10.68.100.23:9000     .minio.sys/new-drive-healing/0/0
```

EXAMPLE

The following command displays the current in-progress S3 API calls on the [[glossary#term-alias|alias]] `myminio`.

```
mc support top locks myminio/
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] support top locks  \
                 [--stale]          \
                 TARGET
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.support.top.locks.TARGET)
Required

The full path to the [[reference/minio-mc/mc-alias-set#minio-mc-alias|alias]] or prefix where the command should run.

--stale[](#mc.support.top.locks.-stale)
Optional

Return only stale locks.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Display the 10 Oldest Locks on the myminio Deployment

```
mc support top locks myminio/
```

### Display Stale Locks on the myminio Deployment

The following command displays all in-progress `s3.PutObject` calls for the `myminio` deployment:

```
mc support top locks --stale myminio/
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).