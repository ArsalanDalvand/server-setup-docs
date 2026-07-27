---
title: "Bucket Versioning — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/object-management/object-versioning.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/object-management/object-versioning"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Bucket Versioning — SILO (MinIO Object Storage)"
---
# Bucket Versioning

[Versioning overview](https://youtu.be/XGOiwV6Cbuk?ref=docs)

[Versioning lab](https://youtu.be/nFUI2N5zH34?ref=docs)

## Overview

MinIO supports keeping multiple “versions” of an object in a single bucket.

When enabled, versioning allows MinIO to keep multiple iterations of the same object.
Write operations which would normally overwrite an existing object instead result in the creation of a new versioned object.
MinIO versioning protects from unintended overwrites and deletions while providing support for “undoing” a write operation.
Bucket versioning is a prerequisite for configuring [[administration/object-management/object-retention#minio-object-locking|object locking and retention rules]].

For versioned buckets, a write operation results in a new version of that object with a unique version ID.
MinIO marks the “latest” version of the object that clients retrieve by default.
Clients can then explicitly choose to list, retrieve, or remove a specific object version.

Define [[administration/object-management/create-lifecycle-management-expiration-rule#minio-lifecycle-management-create-expiry-rule|object expiration]] rules to remove versions of objects no longer needed, such as by the number of versions or the date of versions.

### Read Operations on Versioned Objects

Review each of the four images in this series to see how MinIO retrieves objects in a versioned bucket.
Use the arrows on either side of the images to navigate from one to the next.

Object with Single Version
[](../../_images/minio-versioning-single-version1.svg)

MinIO adds a unique version ID to each object as part of write operations.

Object with Multiple Versions
[](../../_images/minio-versioning-multiple-versions1.svg)

MinIO retains all versions of an object and marks the most recent
version as the “latest”.

Retrieving the Latest Object Version
[](../../_images/minio-versioning-retrieve-latest-version1.svg)

A read operation request without a version ID returns the latest version of the object.

Retrieving a Specific Object Version
[](../../_images/minio-versioning-retrieve-single-version1.svg)

Include the version ID to retrieve a specific version of an object during a read operation.

Changed in version MinIO: Server RELEASE.2023-05-04T21-44-30Z

MinIO does not create versions for creation, mutation, or deletion of explicit directory objects (“prefixes”).
Objects created within that explicit directory object retain normal versioning behavior.

MinIO implicitly determines prefixes from object paths.
Explicit prefix creation typically only occurs with Spark and similar workloads which apply legacy POSIX/HDFS directory creation behavior within the S3 context.

### Versioning is Per-Namespace

MinIO uses the full namespace (the bucket and path to an object) for each object
as part of determining object uniqueness. For example, all of the following
namespaces are “unique” objects, where mutations of each object result in
the creation of new object versions at that namespace:

```
databucket/object.blob
databucket/blobs/object.blob
blobbucket/object.blob
blobbucket/blobs/object.blob
```

While `object.blob` might be the same binary across all namespaces,
MinIO only enforces versioning with a specific namespace and therefore
considers each `object.blob` above as distinct and unique.

### Versioning and Storage Capacity

MinIO does not perform incremental or differential-type versioning. For
mutation-heavy workloads, this may result in substantial drive usage by
older or aged object versions.

For example, consider a 1GB object containing log data. An application
appends 100MB of data to the log and uploads to MinIO. MinIO would then contain
both the 1GB and 1.1GB versions of the object. If the application repeated
this process every day for 10 days, the bucket would eventually contain more
than 14GB of data associated to a single object.

MinIO supports configuring configuring [[administration/object-management/object-lifecycle-management#minio-lifecycle-management|object lifecycle management rules]] to automatically expire or transition aged
object versions and free up storage capacity. For example, you can configure
a rule to automatically expire object versions 90 days after they become
non-current (i.e. no longer the “latest” version of that object). See
[[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|MinIO Object Expiration]] for
more information.

You can alternatively perform manual removal of object versions using the
following commands:

[[reference/minio-mc/mc-rm#mc.rm.-versions|`mc rm --versions`]] - Removes all versions of an object.

[[reference/minio-mc/mc-rm#mc.rm.-older-than|`mc rm --versions --older-than`]] -

Removes all versions of an object older than the specified calendar date.

New in version RELEASE.2024-04-18T19-09-19Z: MinIO emits a warning if the cumulative size of versions for any single object exceeds 1TiB.

### Version ID Generation

MinIO generates a unique and immutable identifier for each versioned object as
part of write operations. Each object version ID consists of a 128-bit
fixed-size [UUIDv4](https://datatracker.ietf.org/doc/html/rfc4122.html#section-4.4). UUID generation is sufficiently
random to ensure high likelihood of uniqueness for any environment, are
computationally difficult to guess, and do not require centralized registration
process and authority to guarantee uniqueness.

[](../../_images/minio-versioning-multiple-versions1.svg)

MinIO does not support client-managed version ID allocation. All version ID
generation is handled by the MinIO server process.

For objects created while versioning is disabled or suspended, MinIO
uses a `null` version ID. You can access or remove these objects by specifying
`null` as the version ID as part of S3 operations.

### Versioned Delete Operations

Performing a `DELETE` operation on a versioned object creates a 0-byte `DeleteMarker` as the latest version of that object.
For objects where the latest version is a `DeleteMarker`, clients must specify versioning flags or identifiers to perform `GET/HEAD/LIST/DELETE` operations on a prior version of that object.
The default server behavior omits `DeleteMarker` objects from consideration for unversioned operations.

MinIO can utilize [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|Lifecycle Management expiration rules]] to automatically remove versioned objects permanently.
Otherwise, use manual `DELETE` operations to permanently remove non-current versioned objects or `DeleteMarker` objects.

MinIO Implements Idempotent Delete Markers

Changed in version RELEASE.2022-08-22T23-53-06Z.

Standard S3 implementations can create multiple sequential delete markers for the same object when processing simple `DeleteObject` requests with no version identifier.
See the S3 docs for details on [managing delete markers](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ManagingDelMarkers.html#RemDelMarker).

MinIO diverges from standard S3 implementation by avoiding this potential duplication of delete markers.
When processing a `Delete` request with no version identifier, MinIO creates at most one Delete Marker for the specified object.
MinIO does not share S3’s behavior in creating multiple sequential delete markers.

To permanently delete an object version, perform the `DELETE` operation and
specify the version ID of the object to delete. Versioned delete operations
are irreversible.

Deleting an Object
[](../../_images/minio-versioning-delete-object1.svg)

Performing a `DELETE` operation on a versioned object produces a
`DeleteMarker` for that object.

Reading a Deleted Object
[](../../_images/minio-versioning-retrieve-deleted-object1.svg)

Clients by default retrieve the “latest” object version. MinIO returns
a `404`-like response if the latest version is a `DeleteMarker`.

Retrieve Previous Version of Deleted Object
[](../../_images/minio-versioning-retrieve-version-before-delete1.svg)

Clients can retrieve any previous version of the object by specifying the
version ID, even if the “Latest” version is a `DeleteMarker`.

Delete a Specific Object Version
[](../../_images/minio-versioning-delete-specific-version1.svg)

Clients can delete a specific object version by specifying the version ID
as part of the `DELETE` operation. Deleting a specific version is
permanent and does not result in the creation of a `DeleteMarker`.

The following [[reference/minio-mc#command-mc|`mc`]] commands operate on `DeleteMarkers` or versioned
objects:

Use [[reference/minio-mc/mc-ls#mc.ls.-versions|`mc ls --versions`]] to view all versions of an object,
including delete markers.

Use [[reference/minio-mc/mc-cp#mc.cp.-version-id|`mc cp --version-id=UUID ...`]] to
retrieve the version of the “deleted” object with matching `UUID`.

Use [[reference/minio-mc/mc-rm#mc.rm.-version-id|`mc rm --version-id=UUID ...`]] to delete
the version of the object with matching `UUID`.

Use [[reference/minio-mc/mc-rm#mc.rm.-versions|`mc rm --versions`]] to delete all versions of an object.

## Tutorials

### Enable Bucket Versioning

You can enable versioning using the MinIO Console, the MinIO [[reference/minio-mc#command-mc|`mc`]] CLI, or
using an S3-compatible SDK.

Use the [[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]] command to enable versioning on an
existing bucket:

```
mc version enable ALIAS/BUCKET
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured
MinIO deployment.

Replace `BUCKET` with the
[[reference/minio-mc/mc-version-enable#mc.version.enable.ALIAS|`target bucket`]] on which to enable
versioning.

Objects created prior to enabling versioning have a `null` [version ID](#minio-bucket-versioning-id).

### Exclude a Prefix From Versioning

You can exclude certain [[administration/concepts#minio-admin-concepts-organize-objects|prefixes]] from versioning using the [[reference/minio-mc#minio-client|MinIO Client]].
This is useful for Spark/Hadoop workloads or others that initially create objects with temporary prefixes.

Replication and Object Locking Require Versioning

MinIO requires versioning to support [[glossary#term-replication|replication]].
Objects in excluded prefixes do not replicate to any peer site or remote site.

MinIO does not support excluding prefixes from versioning on buckets with [[administration/object-management/object-retention#minio-object-locking|object locking enabled]].

Use [[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]] with the [[reference/minio-mc/mc-version-enable#mc.version.enable.-excluded-prefixes|`--excluded-prefixes`]] option:

```
mc version enable --excluded-prefixes "prefix1, prefix2" ALIAS/BUCKET
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

Replace `BUCKET` with the name of the [bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html) you want to exclude [[administration/concepts#minio-admin-concepts-organize-objects|prefixes]] for.

The list of [[reference/minio-mc/mc-version-enable#mc.version.enable.-excluded-prefixes|`--excluded-prefixes`]] prefixes match all objects containing the specified strings in their prefix or name, similar to a regular expression of the form `prefix*`.
To match objects by prefix only, use `prefix/*`.

For example, the following command excludes any objects containing `_test` or `_temp` in their prefix or name from versioning:

```
mc version enable --excluded-prefixes "_test, _temp" local/my-bucket
```

You can exclude up to 10 prefixes for each bucket.
To add or remove prefixes, repeat the [[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]] command with an updated list.
The new list of prefixes replaces the previous one.

To view the currently excluded prefixes, use [[reference/minio-mc/mc-version-info#command-mc.version.info|`mc version info`]] with the `--json` option:

```
mc version info ALIAS/BUCKET --json
```

The command output resembles the following, with the list of excluded prefixes in the `ExcludedPrefixes` property:

```
$ mc version info local/my-bucket --json
{
 "Op": "info",
 "status": "success",
 "url": "local/my-bucket",
 "versioning": {
  "status": "Enabled",
  "MFADelete": "",
  "ExcludedPrefixes": [
   "prefix1, prefix2"
  ]
 }
}
```

To disable prefix exclusion and resume versioning all prefixes, repeat the [[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]] command without [[reference/minio-mc/mc-version-enable#mc.version.enable.-excluded-prefixes|`--excluded-prefixes`]]:

```
mc version enable ALIAS/BUCKET
```

### Exclude Folders from Versioning

You can exclude folders from versioning using the [[reference/minio-mc#minio-client|MinIO Client]].

Replication and Object Locking Require Versioning

MinIO requires versioning to support [[glossary#term-replication|replication]].
Objects in excluded folders do not replicate to any peer site or remote site.

MinIO does not support excluding folders from versioning on buckets with [[administration/object-management/object-retention#minio-object-locking|object locking enabled]].

Object locking

Buckets with [[administration/object-management/object-retention#minio-object-locking|object locking enabled]] require versioning and do not support excluding folders.

Use [[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]] with the [[reference/minio-mc/mc-version-enable#mc.version.enable.-exclude-folders|`--exclude-folders`]] option to exclude objects with names ending in `/` from versioning:

```
mc version enable --exclude-folders ALIAS/BUCKET
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

Replace `BUCKET` with the [bucket](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingBucket.html) you want to exclude [[administration/concepts#minio-admin-concepts-organize-objects|folders]] for.

To check whether folders are versioned for a bucket, use the [[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]] command with the `--json` option.
If the `ExcludeFolders` property is `true`, folders in that bucket are not versioned.

```
mc version enable --excluded-prefixes ALIAS/BUCKET --json
```

The command output resembles the following:

```
$ mc version info local/my-bucket --json
{
 "Op": "info",
 "status": "success",
 "url": "local/my-bucket",
 "versioning": {
  "status": "Enabled",
  "MFADelete": "",
  "ExcludeFolders": true
 }
}
```

To disable folder exclusion and resume versioning all folders, repeat the [[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]] command without [[reference/minio-mc/mc-version-enable#mc.version.enable.-exclude-folders|`--exclude-folders`]]:

```
mc version enable ALIAS/BUCKET
```

### Suspend Bucket Versioning

You can suspend bucket versioning at any time using he MinIO [[reference/minio-mc#command-mc|`mc`]] CLI or using an S3-compatible SDK.

Use the [[reference/minio-mc/mc-version-suspend#command-mc.version.suspend|`mc version suspend`]] command to enable versioning on an existing bucket:

```
mc version suspend ALIAS/BUCKET
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured
MinIO deployment.

Replace `BUCKET` with the
[[reference/minio-mc/mc-mb#mc.mb.ALIAS|`target bucket`]] on which to disable
versioning.

Objects created while versioning is suspended are assigned a `null` [version ID](#minio-bucket-versioning-id).
Any mutations to an object while versioning is suspended result in overwriting that `null` versioned object.
MinIO does not remove or otherwise alter existing versioned objects as part of suspending versioning.
Clients can continue interacting with any existing object versions in the bucket.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).