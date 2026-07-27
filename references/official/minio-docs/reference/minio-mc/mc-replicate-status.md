---
title: "mc replicate status — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-replicate-status.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-replicate-status"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc replicate status — SILO (MinIO Object Storage)"
---
# mc replicate status

## Syntax

The [`mc replicate status`](#command-mc.replicate.status) command displays the [[administration/bucket-replication#minio-bucket-replication-serverside|replication status]] of a MinIO bucket.
The status also lists the remote target path or location.

EXAMPLE

The following command displays the current replication status of the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc replicate status myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] replicate status TARGET
                           [--limit-upload value]
                           [--limit-download value]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.replicate.status.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment and full path to the bucket or bucket prefix for which to display the replication status.
For example:

```
mc replicate status myminio/mybucket
```

--limit-download[](#mc.replicate.status.-limit-download)
Optional

Limit download rates to no more than a specified rate in KiB/s, MiB/s, or GiB/s.
Valid units include:

`B` for bytes

`K` for kilobytes

`G` for gigabytes

`T` for terabytes

`Ki` for kibibytes

`Gi` for gibibytes

`Ti` for tebibytes

For example, to limit download rates to no more than 1 GiB/s, use the following:

```
--limit-download 1G
```

If not specified, MinIO uses an unlimited download rate.

--limit-upload[](#mc.replicate.status.-limit-upload)
Optional

Limit upload rates to no more than the specified rate in KiB/s, MiB/s, or GiB/s.
Valid units include:

`B` for bytes

`K` for kilobytes

`G` for gigabytes

`T` for terabytes

`Ki` for kibibytes

`Gi` for gibibytes

`Ti` for tebibytes

For example, to limit upload rates to no more than 1 GiB/s, use the following:

```
--limit-upload 1G
```

If not specified, MinIO uses an unlimited upload rate.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Display Replication Status

Use [`mc replicate status`](#command-mc.replicate.status) to show bucket replication status:

```
mc replicate status ALIAS/PATH
```

Replace [`ALIAS`](#mc.replicate.status.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`PATH`](#mc.replicate.status.ALIAS) with the path to the bucket or bucket prefix.

## Behavior

### Removed and Re-added ARNs

Changed in version mc: RELEASE.2023-03-20T17-17-53Z

The standard output of this command does not display ARNs previously removed from a replication configuration.

To list all ARNs, including ARNs no longer part of the replication, use the `--json` flag.
The `json` output continues to show data replicated under old ARNs.
This may be valuable if an ARN was removed and re-added for the same bucket.

New ARNs do not cause re-replication of previously synced objects.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).