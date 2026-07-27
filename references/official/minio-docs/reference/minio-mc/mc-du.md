---
title: "mc du — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-du.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-du"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc du — SILO (MinIO Object Storage)"
---
# mc du

## Syntax

The [`mc du`](#command-mc.du) command summarizes the disk usage of buckets and folders.
You can also use [`du`](#command-mc.du) against the local filesystem to produce similar results as the `du` command.

EXAMPLE

The following command prints the disk usage of the `mybucket` bucket on the `myminio` MinIO deployment:

```
mc du play/mybucket
```

The output resembles the following:

```
825KiB 3 objects        mybucket
```

SYNTAX

The [`mc du`](#command-mc.du) command has the following syntax:

```
mc [GLOBALFLAGS] du                    \
                 [--depth]             \
                 [--recursive]         \
                 [--rewind]            \
                 [--versions]          \
                 ALIAS [ALIAS ...]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.du.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment and the full path to the folder. For example:

```
mc du myminio/mybucket
```

You can specify multiple buckets and folders on the same or different MinIO deployment. For example:

```
mc du myminio/mybucket myminio/myotherbucket/myfolder
```

For a folder on a local filesystem, specify the full path to that folder. For example:

```
mc du ~/data/images
```

The time required for [`mc du`](#command-mc.du) to complete depends on the size of the target buckets and folders. A large bucket may take some time to generate a disk usage summary.

--depth, d[](#mc.du.-depth)
Optional

Print the total for all folders N or fewer levels below the path specified in the command. Default is 0, for the specified path only.

--recursive, r[](#mc.du.-recursive)
Optional

Recursively print the total for each bucket or child folder.

--rewind[](#mc.du.-rewind)
Optional

Directs [`mc du`](#command-mc.du) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.du.-rewind) requires that the specified [`ALIAS`](#mc.du.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Use [`--rewind`](#mc.du.-rewind) and [`--versions`](#mc.du.-versions) together to show the disk usage for those object versions which existed at a specific point in time.

--versions[](#mc.du.-versions)
Optional

Directs [`mc du`](#command-mc.du) to operate on all object versions that exist in the
bucket.

[`--versions`](#mc.du.-versions) requires that the specified [`ALIAS`](#mc.du.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Use [`--versions`](#mc.du.-versions) and [`--rewind`](#mc.du.-rewind) together to show the disk usage for those object versions which existed at a specific point in time.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### View the Disk Usage for a Bucket or Folder

Use [`mc du`](#command-mc.du) to print a summary of the disk usage for a bucket or folder:

```
mc du ALIAS/PATH
```

Replace `ALIAS` with the  [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace `PATH` with the path to the bucket or folder on the S3-compatible host.

### View the Disk Usage at a Point-In-Time

Use [`mc du --rewind`](#mc.du.-rewind) to print a summary of disk usage at a specific point-in-time in the past:

```
mc du --rewind DURATION ALIAS/PATH
```

Replace `DURATION` with the desired point-in-time in the past. For example, specify `30d` to show the disk usage 30 days prior to the current date.

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace `PATH` with the path to the bucket or folder on the S3-compatible host.

Requires Versioning

[`mc du`](#command-mc.du) requires [[administration/object-management/object-versioning#minio-bucket-versioning|bucket versioning]] to
use this feature. Use [[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable versioning on a bucket.

### View the Disk Usage Recursively

Use [`mc du --recursive`](#mc.du.-recursive) to print a summary for each folder recursively:

```
mc du --recursive ALIAS/PATH
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace `PATH` with the path to the bucket or folder on the S3-compatible host.

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