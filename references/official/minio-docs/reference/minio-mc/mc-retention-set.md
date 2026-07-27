---
title: "mc retention set — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-retention-set.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-retention-set"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc retention set — SILO (MinIO Object Storage)"
---
# mc retention set

## Syntax

The [`mc retention set`](#command-mc.retention.set) command configures the
[[administration/object-management/object-retention#minio-object-locking|Write-Once Read-Many (WORM) locking]] settings for
an object or object(s) in a bucket. You can also set the default object lock
settings for a bucket, where all objects without explicit object lock settings
inherit the bucket default.

To lock an object under [[administration/object-management/object-retention#minio-object-locking-legalhold|legal hold]],
use [[reference/minio-mc/mc-legalhold-set#command-mc.legalhold.set|`mc legalhold set`]].

[`mc retention set`](#command-mc.retention.set) requires that the specified bucket has object locking
enabled. You can only enable object locking at bucket creation. See
[[reference/minio-mc/mc-mb#mc.mb.-with-lock|`mc mb --with-lock`]] for documentation on creating buckets with
object locking enabled.

EXAMPLE

The following command sets a default 30 day
[[administration/object-management/object-retention#minio-object-locking-governance|GOVERNANCE]] object lock on the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc retention set --default GOVERNANCE "30d" myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] retention set                         \
                 [--bypass]                            \
                 [--default]                           \
                 [--recursive]                         \
                 [--rewind "string"]                   \
                 [--versions]                          \
                 [--version-id "string"]*              \
                 MODE                                  \
                 "VALIDITY"                            \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

[`mc retention set --version-id`](#mc.retention.set.-version-id) is mutually exclusive with
multiple other parameters. See the reference documentation for more
information.

### Parameters

MODE[](#mc.retention.set.MODE)
Required

Sets the locking mode for the [`ALIAS`](#mc.retention.set.ALIAS).
Specify one of the following supported values:

`governance`

`compliance`

See the AWS S3 documentation on [Object Lock Overview](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-overview.html) for more information on the supported
modes.

Requires specifying [`VALIDITY`](#mc.retention.set.VALIDITY).

VALIDITY[](#mc.retention.set.VALIDITY)
Required

The duration which objects remain in the specified
[`MODE`](#mc.retention.set.MODE) after creation.

For days, specify a string formatted as `Nd`. For example,

`30d` for 30 days after object creation.

For years, specify a string formatted as `Ny`. For example,

`1y` for 1 year after object creation.

ALIAS[](#mc.retention.set.ALIAS)
Required

The full path to the object or objects for which to set object lock
configuration. Specify the [[reference/minio-mc/mc-alias-set#alias|alias]] for the MinIO or
S3-compatible service and the full path to bucket. For
example:

```
mc retention set play/mybucket/object.txt MODE VALIDITY
```

If the `ALIAS` specifies a bucket or bucket prefix, include
[`--recursive`](#mc.retention.set.-recursive) to apply the object lock
settings to the bucket contents.

[`mc retention set`](#command-mc.retention.set) by default applies to only the latest object
version. Use [`--version-id`](#mc.retention.set.-version-id) or
[`--versions`](#mc.retention.set.-versions) to apply the object lock
settings to a specific version or to all versions of the object
respectively.

--bypass[](#mc.retention.set.-bypass)
Optional

Allows a user with the `s3:BypassGovernanceRetention` permission
to modify the object. Requires the `governance` retention
[`MODE`](#mc.retention.set.MODE)

--default[](#mc.retention.set.-default)
Optional

Sets the default object lock settings for the bucket specified to
[`ALIAS`](#mc.retention.set.ALIAS) using the
[`MODE`](#mc.retention.set.MODE) and [`VALIDITY`](#mc.retention.set.VALIDITY).
Any objects created in the bucket inherit the default object lock settings
unless explicitly overriden using [`mc retention set`](#command-mc.retention.set).

If specifying [`--default`](#mc.retention.set.-default),
[`mc retention set`](#command-mc.retention.set) ignores all other flags.

--recursive, --r[](#mc.retention.set.-recursive)
Optional

Recursively applies the object lock settings to all objects in the
specified [`ALIAS`](#mc.retention.set.ALIAS) path.

Mutually exclusive with [`--version-id`](#mc.retention.set.-version-id).

--rewind[](#mc.retention.set.-rewind)
Optional

Directs [`mc retention set`](#command-mc.retention.set) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.retention.set.-rewind) requires that the specified [`ALIAS`](#mc.retention.set.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

--version-id, --vid[](#mc.retention.set.-version-id)
Optional

Directs [`mc retention set`](#command-mc.retention.set) to operate only on the specified object version.

[`--version-id`](#mc.retention.set.-version-id) requires that the specified [`ALIAS`](#mc.retention.set.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Mutually exclusive with any of the following flags:

[`--versions`](#mc.retention.set.-versions)

[`--rewind`](#mc.retention.set.-rewind)

[`--recursive`](#mc.retention.set.-recursive)

--versions[](#mc.retention.set.-versions)
Optional

Directs [`mc retention set`](#command-mc.retention.set) to operate on all object versions that exist in the
bucket.

[`--versions`](#mc.retention.set.-versions) requires that the specified [`ALIAS`](#mc.retention.set.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Use [`--versions`](#mc.retention.set.-versions) and
[`--rewind`](#mc.retention.set.-rewind) together to apply the
retention settings to all object versions that existed at a
specific point-in-time.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Set Default Bucket Retention Settings

Use [`mc retention set`](#command-mc.retention.set) with the
[`--recursive`](#mc.retention.set.-recursive) and
[`--default`](#mc.retention.set.-default) to set the default bucket
retention settings.

```
mc retention set  --recursive --default MODE DURATION ALIAS/PATH
```

Replace [`MODE`](#mc.retention.set.MODE) with the retention mode to
enable. MinIO supports the AWS S3 retention modes `governance` and
`compliance`.

Replace [`DURATION`](#mc.retention.set.VALIDITY) with the duration which
the object lock should remain in effect. For example, to set a retention
period of 30 days, specify `30d`.

Replace [`ALIAS`](#mc.retention.set.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.retention.set.ALIAS) with the path to the bucket.

The bucket must have object locking enabled to use this command.
You can only enable object locking when creating a bucket. See
[[reference/minio-mc/mc-mb#mc.mb.-with-lock|`mc mb --with-lock`]] for more information on creating buckets
with object locking enabled.

### Set Object Lock Configuration for Versioned Object

Specific Version

Use [`mc retention set`](#command-mc.retention.set) with
[`--version-id`](#mc.retention.set.-version-id) to apply the retention
settings to a specific object version:

```
mc retention set --version-id VERSION MODE DURATION ALIAS/PATH
```

Replace [`VERSION`](#mc.retention.set.-version-id) with the version
of the object.

Replace [`MODE`](#mc.retention.set.MODE) with the retention mode
to enable. MinIO supports the AWS S3 retention modes `governance` and
`compliance`.

Replace [`DURATION`](#mc.retention.set.VALIDITY) with the duration
which the object lock should remain in effect. For example, to set a
retention period of 30 days, specify `30d`.

Replace [`ALIAS`](#mc.retention.set.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.retention.set.ALIAS) with the path to the
object.

All Versions

Use [`mc retention set`](#command-mc.retention.set) with
[`--versions`](#mc.retention.set.-versions) to apply the retention
settings to a specific object version:

```
mc retention set --versions  MODE DURATION ALIAS/PATH
```

Replace [`MODE`](#mc.retention.set.MODE) with the retention mode
to enable. MinIO supports the AWS S3 retention modes `governance` and
`compliance`.

Replace [`DURATION`](#mc.retention.set.VALIDITY) with the duration
which the object lock should remain in effect. For example, to set a
retention period of 30 days, specify `30d`.

Replace [`ALIAS`](#mc.retention.set.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.retention.set.ALIAS) with the path to the
object.

The bucket must have object locking enabled to use this command.
You can only enable object locking when creating a bucket. See
[[reference/minio-mc/mc-mb#mc.mb.-with-lock|`mc mb --with-lock`]] for more information on creating buckets
with object locking enabled.

## Behavior

### Retention of Object Versions

For buckets with [[reference/minio-mc/mc-version#command-mc.version|`versioning enabled`]], [`mc retention set`](#command-mc.retention.set)
by default operates on the latest version of the target object or object(s).
[`mc retention set`](#command-mc.retention.set) includes specific options that when explicitly
specified direct the command to operate on either a specific object version or
all versions of an object:

Specific Object Version

To direct [`mc retention set`](#command-mc.retention.set) to operate on a specific version of an
object, include the `--version-id` argument:

[`mc retention set --version-id`](#mc.retention.set.-version-id)

[`mc retention set --version-id`](#mc.retention.set.-version-id)

[`mc retention set --version-id`](#mc.retention.set.-version-id)

All Object Versions

To direct [`mc retention set`](#command-mc.retention.set) to operate on all versions of an object,
include the `--versions` argument:

[`mc retention set --versions`](#mc.retention.set.-versions)

[`mc retention set --versions`](#mc.retention.set.-versions)

[`mc retention set --versions`](#mc.retention.set.-versions)

### Interaction with Legal Holds

Locking an object prevents any modification or deletion of that
object, similar to the [`COMPLIANCE`](#mc.retention.set.MODE) object
locking mode. Objects can have simultaneous retention-based locks and
legal hold locks.

The legal hold lock overrides any retention locking, such that an object under
legal hold remains locked even if the retention period expires. Setting,
modifying, or clearing retention settings for an object under legal hold has no
effect until the legal hold either expires or is explicitly disabled.

For more information on object legal holds, see [[reference/minio-mc/mc-legalhold#command-mc.legalhold|`mc legalhold`]].

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