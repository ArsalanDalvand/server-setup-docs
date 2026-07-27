---
title: "mc tag set — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-tag-set.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-tag-set"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc tag set — SILO (MinIO Object Storage)"
---
# mc tag set

## Syntax

The [`mc tag set`](#command-mc.tag.set) command sets one or more tags to a bucket or object.

MinIO supports adding up to 10 custom tags to an object.

EXAMPLE

The following command sets tags for the `mydata` bucket on the
`myminio` MinIO deployment:

```
mc tag set myminio/mydata "tag1=value1&tag2=value2"
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] tag set                   \
                 [--rewind "string"]       \
                 [--versions]              \
                 [--version-id "string"]*  \
                 ALIAS                     \
                 "TAGS"
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

[`mc tag set --version-id`](#mc.tag.set.-version-id) is mutually exclusive with
multiple parameters. See the reference documentation for more information.

### Parameters

ALIAS[](#mc.tag.set.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] for a MinIO deployment and the
full path to the object on which to apply the tag (e.g. bucket and path to
object). For example:

```
mc tag set myminio/mybucket/object.txt
```

TAGS[](#mc.tag.set.TAGS)
Required

An ampersand-seperated (`&`) list of key-value pairs
(`KEY=VALUE`), where each pair represents one tag to assign to the object.
For example:

```
mc tag set myminio/mybucket/object.txt "key1=value1&key2=value2"
```

--exclude-folders[](#mc.tag.set.-exclude-folders)
Optional

New in version RELEASE.2024-01-11T05-49-32Z.

When used with [`--recursive`](#mc.tag.set.-recursive), causes [`mc tag set`](#command-mc.tag.set) to not traverse child prefixes.
Tags are only applied to objects at the specified path.
Requires [`--recursive`](#mc.tag.set.-recursive).

The following example applies the tag `destination=international` to objects at `vacation-photos/cancun/` but not `vacation-photos/cancun/ocean/` or other prefixes.

For example, the above would add the tags to the object at``vacation-photos/cancun/pretty-beach.jpg`` but not to the object at``vacation-photos/cancun/ocean/tropical-fish.jpg``.

```
mc tag set myminio/vacation-photos/cancun "destination=international" --exclude-folders --recursive
```

--recursive, r[](#mc.tag.set.-recursive)
Optional

New in version RELEASE.2023-05-04T18-10-16Z.

Recursively applies the tag to all objects at the path specified to [`ALIAS`](#mc.tag.set.ALIAS).

--rewind[](#mc.tag.set.-rewind)
Optional

Directs [`mc tag set`](#command-mc.tag.set) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.tag.set.-rewind) requires that the specified [`ALIAS`](#mc.tag.set.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

--versions[](#mc.tag.set.-versions)
Optional

Directs [`mc tag set`](#command-mc.tag.set) to operate on all object versions that exist in the
bucket.

[`--versions`](#mc.tag.set.-versions) requires that the specified [`ALIAS`](#mc.tag.set.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Use [`--versions`](#mc.tag.set.-versions) and
[`--rewind`](#mc.tag.set.-rewind) together to apply the tag all object
versions which existed at a specific point in time.

--version-id, --vid[](#mc.tag.set.-version-id)
Optional

Directs [`mc tag set`](#command-mc.tag.set) to operate only on the specified object version.

[`--version-id`](#mc.tag.set.-version-id) requires that the specified [`ALIAS`](#mc.tag.set.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Mutually exclusive with the following parameters:

[`--rewind`](#mc.tag.set.-rewind)

[`--versions`](#mc.tag.set.-versions)

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Apply Tags to a Bucket or Object

Use [`mc tag set`](#command-mc.tag.set) to apply tags to a bucket or object:

```
mc tag set ALIAS/PATH "TAGS"
```

Replace [`ALIAS`](#mc.tag.set.ALIAS) with the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

Replace [`PATH`](#mc.tag.set.ALIAS) with the path to the bucket
or object on the MinIO deployment.

Replace [`TAGS`](#mc.tag.set.TAGS) with one or more ampersand-separated
(`&`) key-value pairs for each tag and its corresponding value.

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