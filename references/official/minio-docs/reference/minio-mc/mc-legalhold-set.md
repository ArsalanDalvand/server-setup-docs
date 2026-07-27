---
title: "mc legalhold set — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-legalhold-set.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-legalhold-set"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc legalhold set — SILO (MinIO Object Storage)"
---
# mc legalhold set

## Syntax

The [`mc legalhold set`](#command-mc.legalhold.set) command enables [[administration/object-management/object-retention#minio-object-locking-legalhold|legal hold]] Write-Once Read-Many (WORM) object locking on
an object or objects.

[[reference/minio-mc/mc-legalhold#command-mc.legalhold|`mc legalhold`]] requires that the specified bucket has
[[administration/object-management/object-retention#minio-object-locking|object locking enabled]]. You can only enable
object locking at bucket creation. See [[reference/minio-mc/mc-mb#mc.mb.-with-lock|`mc mb --with-lock`]] for
documentation on creating buckets with object locking enabled.

EXAMPLE

The following command enables legalhold WORM locking on all existing objects
in the `mydata` bucket on the `myminio` MinIO deployment:

```
mc legalhold set --recursive myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] legalhold set  \
                 [--recursive]  \
                 [--rewind]     \
                 [--version-id] \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.legalhold.set.ALIAS)
Required

The MinIO [[reference/minio-mc/mc-alias-set#alias|alias]] and path to the object or
objects on which to enable the legal hold. For example:

```
mc legalhold set play/mybucket/myobjects/objects.txt
```

--recursive, r[](#mc.legalhold.set.-recursive)
Optional

Applies the legal hold to all existing objects in the
[`ALIAS`](#mc.legalhold.set.ALIAS) bucket or bucket prefix.

`--recursive` only applies to existing objects

To enable legal hold for future objects, periodically repeat the [[reference/minio-mc/mc-legalhold#command-mc.legalhold|`mc legalhold`]] command as new objects are created.

--rewind[](#mc.legalhold.set.-rewind)
Optional

Directs [`mc legalhold set`](#command-mc.legalhold.set) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.legalhold.set.-rewind) requires that the specified [`ALIAS`](#mc.legalhold.set.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

--version-id, vid[](#mc.legalhold.set.-version-id)
Optional

Directs [`mc legalhold set`](#command-mc.legalhold.set) to operate only on the specified object version.

[`--version-id`](#mc.legalhold.set.-version-id) requires that the specified [`ALIAS`](#mc.legalhold.set.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

Use [`mc legalhold set`](#command-mc.legalhold.set) to enable legal hold on objects:

```
mc legalhold set [--recursive] ALIAS/PATH
```

Replace [`ALIAS`](#mc.legalhold.set.ALIAS) with the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the S3-compatible host.

Replace [`PATH`](#mc.legalhold.set.ALIAS) with the path to the bucket
or object on the S3-compatible host. If specifying the path to a bucket or
bucket prefix, include the [`--recursive`](#mc.legalhold.set.-recursive)
option.

## Behavior

### Legal Holds Require Explicit Removal

Legal holds are indefinite and enforce complete immutability for locked objects.
Only privileged users with the [[administration/identity-access-management/policy-based-access-control#policy-action.s3-PutObjectLegalHold|`s3:PutObjectLegalHold`]] can set or
lift the legal hold.

### Legal Holds Complement Other Retention Modes

Legal holds are complementary to both [[administration/object-management/object-retention#minio-object-locking-governance|GOVERNANCE Mode]] and
[[administration/object-management/object-retention#minio-object-locking-compliance|COMPLIANCE Mode]] retention settings. An object held under
both legal hold and a `GOVERNANCE/COMPLIANCE` retention rule remains WORM
locked until the legal hold is lifed and the rule expires.

For `GOVERNANCE` locked objects, the legal hold prevents mutating the object
even if the user has the necessary privileges to bypass retention.

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