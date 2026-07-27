---
title: "mc legalhold clear — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-legalhold-clear.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-legalhold-clear"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc legalhold clear — SILO (MinIO Object Storage)"
---
# mc legalhold clear

## Syntax

The [`mc legalhold clear`](#command-mc.legalhold.clear) command removes the current [[administration/object-management/object-retention#minio-object-locking-legalhold|legal hold]] setting for an object or objects.

Removing the legal hold on object(s) does not remove any other
[[administration/object-management/object-retention#minio-object-locking-governance|GOVERNANCE Mode]] and
[[administration/object-management/object-retention#minio-object-locking-compliance|COMPLIANCE Mode]] retention settings in place for
the object(s)

EXAMPLE

The following command removes the legal hold on all objects in the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc legalhold clear --recursive myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] legalhold clear \
                 [--recursive]   \
                 [--rewind]      \
                 [--version-id]  \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.legalhold.clear.ALIAS)
Required

The MinIO [[reference/minio-mc/mc-alias-set#alias|alias]] and path to the object or
objects on which to remove the legal hold. For example:

```
mc legalhold clear play/mybucket/myobjects/objects.txt
```

--recursive, r[](#mc.legalhold.clear.-recursive)
Optional

Removes the legal hold on all objects in the
[`ALIAS`](#mc.legalhold.clear.ALIAS) bucket or bucket prefix.

--rewind[](#mc.legalhold.clear.-rewind)
Optional

Directs [`mc legalhold clear`](#command-mc.legalhold.clear) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.legalhold.clear.-rewind) requires that the specified [`ALIAS`](#mc.legalhold.clear.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

--version-id, vid[](#mc.legalhold.clear.-version-id)
Optional

Directs [`mc legalhold clear`](#command-mc.legalhold.clear) to operate only on the specified object version.

[`--version-id`](#mc.legalhold.clear.-version-id) requires that the specified [`ALIAS`](#mc.legalhold.clear.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Retrieve the Legal Hold Status Objects

Use [`mc legalhold clear`](#command-mc.legalhold.clear) to retrieve the legal hold status of an object.
Include [`--recursive`](#mc.legalhold.clear.-recursive) to return the legal hold
status of the contents of a bucket:

```
mc legalhold clear [--recursive] ALIAS/PATH
```

Replace [`ALIAS`](#mc.legalhold.clear.ALIAS) with the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the S3-compatible host.

Replace [`PATH`](#mc.legalhold.clear.ALIAS) with the path to the bucket
or object on the S3-compatible host. If specifying the path to a bucket or
bucket prefix, include the [`--recursive`](#mc.legalhold.clear.-recursive)
option.

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