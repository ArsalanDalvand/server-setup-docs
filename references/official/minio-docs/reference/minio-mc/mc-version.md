---
title: "mc version — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-version.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-version"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc version — SILO (MinIO Object Storage)"
---
# mc version

## Description

The [`mc version`](#command-mc.version) commands enable, disable, and retrieve the [[administration/object-management/object-versioning#minio-bucket-versioning|versioning]] status for a MinIO bucket.

For more information about object versioning in MinIO, see [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]].

[`mc version`](#command-mc.version) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-version-enable#command-mc.version.enable|`enable`]]

The [[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]] command enables versioning on the specified bucket.

[[reference/minio-mc/mc-version-info#command-mc.version.info|`info`]]

The [[reference/minio-mc/mc-version-info#command-mc.version.info|`mc version info`]] command returns the versioning status for the specified bucket.

[[reference/minio-mc/mc-version-suspend#command-mc.version.suspend|`suspend`]]

The [[reference/minio-mc/mc-version-suspend#command-mc.version.suspend|`mc version suspend`]] command disables versioning on the specified bucket.

## Behavior

### Object Locking Enables Bucket Versioning

While bucket versioning is disabled by default, configuring object locking on a bucket or an object in that bucket automatically enables versioning for the bucket.
See [[reference/minio-mc/mc-retention#command-mc.retention|`mc retention`]] for more information on configuring object locking.

### Bucket Versioning with Existing Data

Enabling bucket versioning on a bucket with existing data immediately creates a null value version ID for each unversioned object.

Disabling bucket versioning on a bucket with existing versioned data does not remove any versioned objects.
Applications can continue to access versioned data after disabling bucket versioning.
Use [[reference/minio-mc/mc-rm#mc.rm.-versions|`mc rm --versions ALIAS/BUCKET/OBJECT`]] to delete an object and all its versions.

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