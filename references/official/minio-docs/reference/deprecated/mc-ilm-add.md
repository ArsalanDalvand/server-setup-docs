---
title: "mc ilm add — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-ilm-add.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-ilm-add"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm add — SILO (MinIO Object Storage)"
---
# mc ilm add

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc ilm add` replaced by [[reference/minio-mc/mc-ilm-rule-add#command-mc.ilm.rule.add|`mc ilm rule add`]].

## Syntax

The [`mc ilm add`](#command-mc.ilm.add) command adds an object lifecycle management rule to a bucket.

The command supports adding both [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-tiering|Transition (Tiering)]] and [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|Expiration]] lifecycle management rules.

EXAMPLE

The following command adds new lifecycle management rules to the `mydata` bucket on the `myminio` deployment:

```
mc ilm add --expire-days 90 --noncurrent-expire-days 30 myminio/mydata

mc ilm add --expire-delete-marker myminio/mydata

mc ilm add --transition-days 30 --transition-tier "COLDTIER" myminio/mydata

mc ilm add --noncurrent-transition-days 7 --noncurrent-transition-tier "COLDTIER"
```

The configured rules have the following effect:

Delete objects more than 90 days old

Delete objects 30 days after they become non-current

Delete `DeleteMarker` tombstones if that object has no other versions remaining.

Transition objects more than 30 days old to the `COLDTIER` remote tier.

Transition objects 7 days after they become non-current to the `COLDTIER` remote tier.

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] ilm add                                    \
                 [--prefix string]                          \
                 [--tags string]                            \
                 --expire-days "integer"                    \
                 [--expire-delete-marker]                   \
                 [--transition-days "string"]               \
                 [--transition-tier "string"]               \
                 [--noncurrent-expire-days "integer"]       \
                 [--noncurrent-expire-newer "integer"]      \
                 [--noncurrent-transition-days "integer"]   \
                 [--noncurrent-transition-tier "string"]    \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.ilm.add.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] and bucket on the MinIO deployment to which to add the object lifecycle management rule.

For example:

```
mc ilm add myminio/mydata
```

--prefix[](#mc.ilm.add.-prefix)
Optional

Restrict the management rule to a specific object prefix.

For example:

```
mc ilm add --prefix "meetingnotes/" myminio/mydata/ --expire-days "90"
```

The command creates a rule that expires objects in the `mydata` bucket of the `myminio` ALIAS after 90 days for any object with the `meetingnotes/` prefix.

--tags[](#mc.ilm.add.-tags)
Optional

One or more ampersand `&`-delimited key-value pairs describing the object tags to use for filtering objects to which the lifecycle configuration rule applies.

This option is mutually exclusive with the following option:

[`--expire-delete-marker`](#mc.ilm.add.-expire-delete-marker)

--expire-days[](#mc.ilm.add.-expire-days)
Required

The number of days to retain an object after being created.
MinIO marks the object for deletion after the specified number of days pass.
Specify the number of days as an integer, e.g. `30` for 30 days.

For versioned buckets, the expiry rule applies only to the current object version.
Use the [`--noncurrent-expire-days`](#mc.ilm.add.-noncurrent-expire-days) option to apply expiration behavior to noncurrent object versions.

MinIO uses a [[operations/concepts/scanner#minio-concepts-scanner|scanner process]] to check objects against all configured lifecycle management rules.
Slow scanning due to high IO workloads or limited system resources may delay application of lifecycle management rules.
See [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-scanner|Lifecycle Management Object Scanner]] for more information.

Mutually exclusive with the following options:

[`--expire-delete-marker`](#mc.ilm.add.-expire-delete-marker)

For more complete documentation on object expiration, see [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|Object Expiration]] and [[administration/object-management/object-delete#minio-object-delete|Object Deletion]].

--expire-delete-marker[](#mc.ilm.add.-expire-delete-marker)
Optional

Specify this option to direct MinIO to remove delete markers for objects with no remaining object versions.
Specifically, the delete marker is the only remaining “version” of the given object.

This option is mutually exclusive with the following option:

[`--tags`](#mc.ilm.add.-tags)

[`--expire-days`](#mc.ilm.add.-expire-days)

MinIO uses a [[operations/concepts/scanner#minio-concepts-scanner|scanner process]] to check objects against all configured lifecycle management rules.
Slow scanning due to high IO workloads or limited system resources may delay application of lifecycle management rules.
See [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-scanner|Lifecycle Management Object Scanner]] for more information.

For more complete documentation on object expiration, see [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|Object Expiration]] and [[administration/object-management/object-delete#minio-object-delete|Object Deletion]].

--transition-days[](#mc.ilm.add.-transition-days)
Optional

The number of calendar days from object creation after which MinIO marks an object as eligible for transition.
MinIO transitions the object to the configured remote tier specified to the [`--transition-tier`](#mc.ilm.add.-transition-tier).
Specify the number of days as an integer, e.g. `30` for 30 days.

For versioned buckets, the transition rule applies only to the current object version.
Use the [`--noncurrent-transition-days`](#mc.ilm.add.-noncurrent-transition-days) option to apply transition behavior to noncurrent object versions.

Requires specifying [`--transition-tier`](#mc.ilm.add.-transition-tier).

MinIO uses a [[operations/concepts/scanner#minio-concepts-scanner|scanner process]] to check objects against all configured lifecycle management rules.
Slow scanning due to high IO workloads or limited system resources may delay application of lifecycle management rules.
See [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-scanner|Lifecycle Management Object Scanner]] for more information.

For more complete documentation on object transition, see [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-tiering|Object Transition (“Tiering”)]].

--transition-tier[](#mc.ilm.add.-transition-tier)
Optional

The remote tier to which MinIO [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-tiering|transition objects]].
Specify an existing remote tier created by [[reference/deprecated/mc-admin-tier#command-mc.admin.tier|`mc admin tier`]].

Required if specifying [`--transition-days`](#mc.ilm.add.-transition-days).

--noncurrent-expire-days[](#mc.ilm.add.-noncurrent-expire-days)
Optional

The number of days to retain an object version after becoming non-current (i.e. a different version of that object is now the HEAD).
MinIO marks noncurrent object versions for deletion after the specified number of days pass.

This option has the same behavior as the S3 `NoncurrentVersionExpiration` action.

MinIO uses a [[operations/concepts/scanner#minio-concepts-scanner|scanner process]] to check objects against all configured lifecycle management rules.
Slow scanning due to high IO workloads or limited system resources may delay application of lifecycle management rules.
See [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-scanner|Lifecycle Management Object Scanner]] for more information.

--noncurrent-transition-days[](#mc.ilm.add.-noncurrent-transition-days)
Optional

The number of days an object has been non-current (i.e. replaced by a newer version of that same object) after which MinIO marks the object version as eligible for transition.
MinIO transitions the object to the configured remote tier specified to the [`--transition-tier`](#mc.ilm.add.-transition-tier) once the system host datetime passes that calendar date.

This option has no effect on non-versioned buckets.
Requires specifying [`--noncurrent-transition-tier`](#mc.ilm.add.-noncurrent-transition-tier).

This option has the same behavior as the S3 `NoncurrentVersionTransition` action.

MinIO uses a [[operations/concepts/scanner#minio-concepts-scanner|scanner process]] to check objects against all configured lifecycle management rules.
Slow scanning due to high IO workloads or limited system resources may delay application of lifecycle management rules.
See [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-scanner|Lifecycle Management Object Scanner]] for more information.

--noncurrent-transition-tier[](#mc.ilm.add.-noncurrent-transition-tier)
Optional

The remote tier to which MinIO [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-tiering|transitions noncurrent objects versions]].
Specify a remote tier created by [[reference/deprecated/mc-admin-tier#command-mc.admin.tier|`mc admin tier`]].

--noncurrent-expire-newer[](#mc.ilm.add.-noncurrent-expire-newer)
Optional

The maximum number of non-current object versions to retain, ordered from newest to oldest.

Use this flag to retain a certain number of past versions of a file in a first in, first out fashion.
After retaining the maximum number of non-current versions, MinIO marks any remaining older non-current object versions as eligible for expiration.

The following table lists a number of object versions and their expiration eligibility based on `--noncurrent-expire-newer 3`:

v5 (current version)

Current version not affected by ILM rules.

v4

retained

v3

retained

v2

retained

v1

marked for expiry

MinIO retains the current version, v5.
MinIO also retains the next `3` non-current versions, starting with the newest.
This means MinIO marks `v4`, `v3`, and `v2` for the three non-current version to retain.

`v1` would be a fourth non-current version, which falls outside the limit of non-current versions to retain, so MinIO marks `v1` for expiration.

Updating the number for this flag only impacts the unmarked versions of objects.
Any versions already marked for expiration do not change if you increase the number to retain.

MinIO uses a [[operations/concepts/scanner#minio-concepts-scanner|scanner process]] to check objects against all configured lifecycle management rules.
Slow scanning due to high IO workloads or limited system resources may delay application of lifecycle management rules.
See [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-scanner|Lifecycle Management Object Scanner]] for more information.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Expire All Bucket Contents After Number of Days

Use [`mc ilm add`](#command-mc.ilm.add) with [`--expire-days`](#mc.ilm.add.-expire-days) to mark bucket contents for expiration after a number of days pass from the object’s creation:

```
mc ilm add ALIAS/PATH --expire-days "DAYS"
```

Replace [`ALIAS`](#mc.ilm.add.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.ilm.add.ALIAS) with the path to the bucket on the S3-compatible host.

Replace [`DATE`](#mc.ilm.add.-expire-days) with the number of days after which to expire the object.
For example, specify `30` to expire the object 30 days after creation.

### Transition Non-Current Object Versions at a Prefix to a Different Tier

Use the [`mc ilm add`](#command-mc.ilm.add) with [`--prefix`](#mc.ilm.add.-prefix) and [`--transition-tier`](#mc.ilm.add.-transition-tier) to transition older non-current versions of an object to a different storage tier.

```
mc ilm add --prefix "doc/" --transition-days "90" --trasition-tier "MINIOTIER-1"                  \
       --noncurrent-transition-days "45" --noncurrent-transition-tier "MINIOTIER-2"  \
       myminio/mybucket/
```

This command looks at the contents with the `doc/` prefix in the `mybucket` bucket on the `myminio` deployment.

Current objects in the prefix older than 90 days move to the `MINIOTIER-1` storage tier.

Non-current objects in the prefix older than 45 days move to the `MINIOTIER-2` storage tier.

Both `MINIOTIER-1` and `MINIOTIER-2` have already been created with [[reference/deprecated/mc-admin-tier#mc.admin.tier.add|`mc admin tier add`]].

### Expire All Objects at a Prefix, Retain Current Object Versions Longer Than Non-Current Object Versions

Use the [`mc ilm add`](#command-mc.ilm.add) command with [`--prefix`](#mc.ilm.add.-prefix), [`--expire-days`](#mc.ilm.add.-expire-days), and [`--noncurrent-expire-days`](#mc.ilm.add.-noncurrent-expire-days) to expire current and non-current versions of an object at different times.

```
mc ilm add --prefix "doc/" --expire-days "300" --noncurrent-expire-days "100" myminio/mybucket/
```

This command looks at the contents with the `doc/` prefix in the `mybucket` bucket on the `myminio` deployment.

Current objects expire after 300 days.

Non-current objects expire after 100 days.

## Behavior

### Lifecycle Management Object Scanner

MinIO uses a [[operations/concepts/scanner#minio-concepts-scanner|scanner process]] to check objects against the configured lifecycle management rules.
Slow scanning due to high IO workloads or limited system resources may delay application of lifecycle management rules.
See [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-scanner|Lifecycle Management Object Scanner]] for more information.

### Expiry vs Transition

MinIO supports specifying both expiry and transition rules in the same
bucket or bucket prefix. MinIO can execute an expiration rule on an object
regardless of its transition status. Use
[[reference/deprecated/mc-ilm-ls#command-mc.ilm.ls|`mc ilm ls`]] to review the currently configured object lifecycle
management rules for any potential interactions between expiry and transition
rules.

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