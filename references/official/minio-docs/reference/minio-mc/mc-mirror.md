---
title: "mc mirror — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-mirror.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-mirror"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc mirror — SILO (MinIO Object Storage)"
---
# mc mirror

## Syntax

The [`mc mirror`](#command-mc.mirror) command synchronizes content to MinIO deployment, similar to the `rsync` utility.
[`mc mirror`](#command-mc.mirror) supports filesystems, MinIO deployments, and other S3-compatible hosts as the synchronization source.

Note

[`mc mirror`](#command-mc.mirror) only synchronizes the current object without any version information or metadata.
To synchronize an object’s version history and metadata, consider using [[reference/minio-mc/mc-replicate#command-mc.replicate|`mc replicate`]] for [[administration/bucket-replication#minio-bucket-replication-serverside|bucket replication]] or [[reference/minio-mc-admin/mc-admin-replicate#command-mc.admin.replicate|`mc admin replicate`]] for [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]].

EXAMPLE

The following command synchronizes content from a local filesystem directory to the `mydata` bucket on the `myminio` MinIO deployment.

```
mc mirror --watch ~/mydata myminio/mydata
```

The command “watches” for files added or removed on the local filesystem and synchronizes those operations to MinIO until explicitly terminated.

[`mc mirror --watch`](#mc.mirror.-watch) updates files changed on the local filesystem to MinIO (see [`--overwrite`](#mc.mirror.-overwrite)).
`--watch` does not remove other files from MinIO not present on the local filesystem (see [`--remove`](#mc.mirror.-remove)).

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] mirror                            \
                 [--active-active]                 \
                 [--attr "string"]                 \
                 [--checksum "value"]              \
                 [--disable-multipart]             \
                 [--dry-run]                       \
                 [--enc-kms "string"]              \
                 [--enc-s3 "string"]               \
                 [--enc-c "string"]                \
                 [--exclude "string"]              \
                 [--exclude-bucket "string"]       \
                 [--exclude-storageclass "string"] \
                 [--limit-download string]         \
                 [--limit-upload string]           \
                 [--md5]                           \
                 [--monitoring-address "string"]   \
                 [--newer-than "string"]           \
                 [--older-than "string"]           \
                 [--overwrite]                     \
                 [--preserve]                      \
                 [--region "string"]               \
                 [--remove]                        \
                 [--retry]                         \
                 [--skip-errors]                   \
                 [--storage-class "string"]        \
                 [--summary]                       \
                 [--watch]                         \
                 SOURCE                            \
                 TARGET
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

SOURCE[](#mc.mirror.SOURCE)
Required

The file(s) or object(s) to synchronize to the [`TARGET`](#mc.mirror.TARGET) S3 host.

For objects on S3-compatible hosts, specify the path to the object as `ALIAS/PATH`, where:

`ALIAS` is the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host, and

`PATH` is the path to the bucket or object. If specifying a bucket, [`mc mirror`](#command-mc.mirror) synchronizes all objects in the bucket.

```
mc mirror [FLAGS] play/mybucket/ myminio/mybucket
```

For files on a filesystem, specify the full filesystem path to the file or directory :

```
mc mirror [FLAGS] ~/data/ myminio/mybucket
```

If specifying a directory, [`mc mirror`](#command-mc.mirror) synchronizes all files in the directory.

TARGET[](#mc.mirror.TARGET)
Required

The full path to bucket to which [`mc mirror`](#command-mc.mirror) synchronizes SOURCE objects. Specify the `TARGET` as `ALIAS/PATH`, where:

`ALIAS` is the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host, and

`PATH` is the path to the bucket.

```
mc mirror SOURCE play/mybucket
```

[`mc mirror`](#command-mc.mirror) uses the object or file names from the [`SOURCE`](#mc.mirror.SOURCE) when synchronizing to the `TARGET` bucket.

--active-active[](#mc.mirror.-active-active)
Optional

Establish active-active mirror activities between two sites.
The command must be repeated on each site.

For example:

On site A, to mirror from A to B

```
mc mirror --active-active siteA siteB
```

On site B, to mirror from B to A

```
mc mirror --active-active siteB siteA
```

--attr[](#mc.mirror.-attr)
Optional

Add custom metadata for mirrored objects. Specify key-value pairs as `KEY=VALUE\;`.
For example, `--attr key1=value1\;key2=value2\;key3=value3`.

--checksum[](#mc.mirror.-checksum)
Optional

New in version RELEASE.2024-10-02T08-27-28Z.

Add a checksum to an uploaded object.

Valid values are:
- `MD5`
- `CRC32`
- `CRC32C`
- `SHA1`
- `SHA256`

The flag requires server trailing headers and works with AWS or MinIO targets.

--disable-multipart[](#mc.mirror.-disable-multipart)
Optional

Disables multipart upload for the synchronization session.

--dry-run[](#mc.mirror.-dry-run)
Optional

Perform a mock mirror operation.
Use this operation to test that the [`mc mirror`](#command-mc.mirror) operation will only mirror the desired objects or buckets.

--enc-kms[](#mc.mirror.-enc-kms)

Encrypt or decrypt objects using server-side [[administration/server-side-encryption#minio-sse|SSE-KMS encryption]] with client-managed keys.

The parameter accepts a key-value pair formatted as `KEY=VALUE`

`KEY`

The full path to the object as `alias/bucket/path/object.ext`.

You can specify only the top-level path to use a single encryption key for all operations in that path.

`VALUE`

Specify an existing data key on the external KMS.

See the [[reference/minio-mc-admin/mc-admin-kms-key#mc.admin.kms.key.create|`mc admin kms key create`]] reference for creating data keys.

For example:

```
--enc-kms "myminio/mybucket/prefix/object.obj=mybucketencryptionkey"
```

You can specify multiple encryption keys by repeating the parameter.

Specify the path to a prefix to apply encryption to all matching objects at that path:

```
--enc-kms "myminio/mybucket/prefix/=mybucketencryptionkey"
```

--enc-s3[](#mc.mirror.-enc-s3)
Optional

Encrypt or decrypt objects using server-side [[administration/server-side-encryption#minio-sse|SSE-S3 encryption]] with KMS-managed keys.
Specify the full path to the object as `alias/bucket/prefix/object`.

For example:

```
--enc-s3 "myminio/mybucket/prefix/object.obj"
```

You can specify the parameter multiple times to denote different object(s) to encrypt:

```
--enc-s3 "myminio/mybucket/foo/fooobject.obj" --enc-s3 "myminio/mybucket/bar/barobject.obj"
```

Specify the path to a prefix to apply encryption to all matching objects at that path:

```
--enc-s3 "myminio/mybucket/foo"
```

--enc-c[](#mc.mirror.-enc-c)
Optional

Encrypt or decrypt objects using server-side [[administration/server-side-encryption#minio-sse|SSE-C encryption]] with client-managed keys.

The parameter accepts a key-value pair formatted as `KEY=VALUE`

`KEY`

The full path to the object as `alias/bucket/path/object.ext`.

You can specify only the top-level path to use a single encryption key for all operations in that path.

`VALUE`

Specify either a 32-byte RawBase64-encoded key or a 64-byte hex-encoded key for use with SSE-C encryption.

Raw Base64 encoding rejects `=`-padded keys.
Omit the padding or use a Base64 encoder that supports RAW formatting.

`KEY` - the full path to the object as `alias/bucket/path/object`.

`VALUE` - the 32-byte RAW Base64-encoded data key to use for encrypting object(s).

For example:

```
# RawBase64-Encoded string "mybucket32byteencryptionkeyssec"
--enc-c "myminio/mybucket/prefix/object.obj=bXlidWNrZXQzMmJ5dGVlbmNyeXB0aW9ua2V5c3NlYwo"
```

You can specify multiple encryption keys by repeating the parameter.

Specify the path to a prefix to apply encryption to all matching objects at that path:

```
--enc-c "myminio/mybucket/prefix/=bXlidWNrZXQzMmJ5dGVlbmNyeXB0aW9ua2V5c3NlYwo"
```

Note

MinIO strongly recommends against using SSE-C encryption in production workloads.
Use SSE-KMS via the `--enc-kms` or SSE-S3 via `--enc-s3` parameters instead.

--exclude[](#mc.mirror.-exclude)
Optional

Exclude object(s) in the [`SOURCE`](#mc.mirror.SOURCE) path that match the specified object [[reference/minio-mc#minio-wildcard-matching|name pattern]].

--exclude-bucket[](#mc.mirror.-exclude-bucket)
Optional

New in version mc: RELEASE.2024-03-03T00-13-08Z

Exclude bucket(s) in the [`SOURCE`](#mc.mirror.SOURCE) path that match the specified bucket [[reference/minio-mc#minio-wildcard-matching|name pattern]].

--exclude-storageclass[](#mc.mirror.-exclude-storageclass)
Optional

Exclude object(s) on the [`SOURCE`](#mc.mirror.SOURCE) that have the specified storage class.
You can use this flag multiple times in a command to exclude objects from more than one storage class.

Use this to exclude objects with storage classes that require rehydration or restoration of objects, such as migrating from an AWS S3 bucket where some objects have the `GLACIER` or `DEEP_ARCHIVE` storage classes.

--limit-download[](#mc.mirror.-limit-download)
Optional

Limit client-side download rates to no more than a specified rate in KiB/s, MiB/s, or GiB/s.
This affects only the download to the local device running the MinIO Client.
Valid units include:

`B` for bytes

`K` for kilobytes

`M` for megabytes

`G` for gigabytes

`T` for terabytes

`Ki` for kibibytes

`Mi` for mibibytes

`Gi` for gibibytes

`Ti` for tebibytes

For example, to limit download rates to no more than 1 GiB/s, use the following:

```
--limit-download 1G
```

If not specified, MinIO uses an unlimited download rate.

--limit-upload[](#mc.mirror.-limit-upload)
Optional

Limit client-side upload rates to no more than the specified rate in KiB/s, MiB/s, or GiB/s.
This affects only the upload from the local device running the MinIO Client.
Valid units include:

`B` for bytes

`K` for kilobytes

`M` for megabytes

`G` for gigabytes

`T` for terabytes

`Ki` for kibibytes

`Mi` for mibibytes

`Gi` for gibibytes

`Ti` for tebibytes

For example, to limit upload rates to no more than 1 GiB/s, use the following:

```
--limit-upload 1G
```

If not specified, MinIO uses an unlimited upload rate.

--md5[](#mc.mirror.-md5)
Optional

Forces all uploads to calculate MD5 checksums.

--monitoring-address[](#mc.mirror.-monitoring-address)
Optional

Creates a [Prometheus](https://prometheus.io/) endpoint for monitoring mirroring activity.
Specify the local network adapter and port address on which to create the scraping endpoint.
Defaults to `localhost:8081`).

--newer-than[](#mc.mirror.-newer-than)
Optional

Mirror object(s) newer than the specified number of days.
Specify a string in `#d#hh#mm#ss` format
For example: `--newer-than 1d2hh3mm4ss`.

--older-than[](#mc.mirror.-older-than)
Optional

Mirror object(s) older than the specified time limit.
Specify a string in `#d#hh#mm#ss` format.
For example: `--older-than 1d2hh3mm4ss`.

Defaults to `0` (all objects).

--overwrite[](#mc.mirror.-overwrite)
Optional

Overwrites object(s) on the [`TARGET`](#mc.mirror.TARGET).

For example, consider an active `mc mirror --overwrite` synchronizing content from Source to Destination.

If an object on Source changes, `mc mirror --overwrite` synchronizes and overwrites any matching file on Destination.

Without `--overwrite`, if an object already exists on the Destination, the mirror process fails to synchronize that object.
`mc mirror` logs an error and continues to synchronize other objects.

--preserve, a[](#mc.mirror.-preserve)
Optional

Preserve file system attributes and bucket policy rules of the [`SOURCE`](#mc.mirror.SOURCE) on the [`TARGET`](#mc.mirror.TARGET).

--region[](#mc.mirror.-region)
Optional

Specify the `string` region when creating new bucket(s) on the target.

Defaults to `"us-east-1"`.

--remove[](#mc.mirror.-remove)
Optional

Removes object(s) on the Target that do not exist on the Source.

Use the `--remove` flag to have the same list of objects on both Source and Target.

For example, objects A, B, and C exist on Source.
Objects C, D, and E exist on Target.

When running `mc mirror --remove`, objects A and B synchronize to Target and objects D and E are removed from Target.
Since an object C already exists on both, nothing moves from Source to Target.

After the action, only objects A, B, and C exist on both the Source and the Target.

`mc mirror --remove` does not verify that the contents of object C are the same on both Source and Target, only that an object called C exists on both.
To ensure objects on the Source and Target match both names and content, use  [`--overwrite`](#mc.mirror.-overwrite) or [`--watch`](#mc.mirror.-watch).

Changed in version RELEASE.2023-05-04T18-10-16Z: `mc mirror --remove` returns an error if the target path is a local filesystem directory that does not exist.

In prior versions, specifying `/path/to/directory` would result in the removal of the `/path/to` folder if `directory` did not exist.

--retry[](#mc.mirror.-retry)
Optional

In case of errors during mirror process, retry on each errored object.

--storage-class, sc[](#mc.mirror.-storage-class)
Optional

Set the storage class for the new object(s) on the [`TARGET`](#mc.mirror.TARGET).

See the Amazon documentation on [Storage Classes](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html) for more information on S3 storage classses.

--skip-errors[](#mc.mirror.-skip-errors)
Optional

New in version mc: RELEASE.2024-01-28T16-23-14Z

Skip any objects that produce errors while mirroring.

--summary[](#mc.mirror.-summary)
Optional

On completion, output a summary of the data that was synchronized.

--watch, w[](#mc.mirror.-watch)
Optional

Use `--watch` flag to mirror objects from Source to Target, where the Target may also have additional objects not present on the Source.

`--watch` continuously synchronizes files from Source to Target until explicitly terminated

The Target may have files that do not exist on Source

`--watch` overwrites objects on the Target if a match exists on Source, like the [`--overwrite`](#mc.mirror.-overwrite) flag

Defaults to `0` (all objects).

For example, object A and B exist on the watched Source.
Objects A, B, and C exist on the watched Target.

A client writes object D to Source and removes object B.

After the operation, objects A and D exist on the Source.
Objects A, C, and D exist on the Target.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Mirror a Local Directory to an S3-Compatible Host

Use [`mc mirror`](#command-mc.mirror) to mirror files from a filesystem to an S3 Host:

```
mc mirror FILEPATH ALIAS/PATH
```

Replace [`FILEPATH`](#mc.mirror.SOURCE) with the full file path to the directory to mirror.

Replace [`ALIAS`](#mc.mirror.TARGET) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.mirror.TARGET) with the destination bucket.

### Continuously Mirror a Local Directory to an S3-Compatible Host

Use [`mc mirror`](#command-mc.mirror) with [`--watch`](#mc.mirror.-watch) to continuously mirror files from a filesystem to an S3-compatible host where objects added to or deleted from the filesystem are added to or deleted from the host:

```
mc mirror --watch FILEPATH ALIAS/PATH
```

Replace [`FILEPATH`](#mc.mirror.SOURCE) with the full file path to the directory to mirror.

Replace [`ALIAS`](#mc.mirror.TARGET) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.mirror.TARGET) with the destination bucket.

### Continuously Mirror S3 Bucket to an S3-Compatible Host

Use [`mc mirror`](#command-mc.mirror) with [`--watch`](#mc.mirror.-watch) to continuously mirror objects in a bucket on one S3-compatible host to another S3-compatible host where objects added to or deleted from the bucket are added to or deleted from the host.

```
mc mirror --watch SRCALIAS/SRCPATH TGTALIAS/TGTPATH
```

Replace [`SRCALIAS`](#mc.mirror.SOURCE) with [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`SRCPATH`](#mc.mirror.SOURCE) with the bucket to mirror.

Replace [`TGTALIAS`](#mc.mirror.TARGET) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`TGTPATH`](#mc.mirror.TARGET) with the destination bucket.

### Mirror Objects from AWS S3 to MinIO Skipping Objects in GLACIER

Use [`mc mirror`](#command-mc.mirror) with [`--exclude-storageclass`](#mc.mirror.-exclude-storageclass) to mirror objects from AWS S3 to MinIO without mirroring objects in GLACIER or DEEP_ARCHIVE storage.

```
mc mirror --exclude-storageclass GLACIER  \
   --exclude-storageclass DEEP_ARCHIVE SRCALIAS/SRCPATH TGALIAS/TGPATH
```

Replace [`SRCALIAS`](#mc.mirror.SOURCE) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3 host.

Replace [`SRCPATH`](#mc.mirror.SOURCE) with the bucket to mirror.

Replace [`TGTALIAS`](#mc.mirror.TARGET) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3 host.

Replace [`TGTPATH`](#mc.mirror.TARGET) with the destination bucket.

## Behavior

### Mirror Continues on Failed Object

If an object of the same name exists on the target, MinIO outputs an error for the duplicate object.
`mc mirror` continues to mirror other objects from the source to the destination after the error.

### MinIO Trims Empty Prefixes on Object Removal

The [`mc mirror --watch`](#mc.mirror.-watch) command continuously synchronizes the source and destination targets for added and deleted objects.
This includes automatically removing objects on the destination if they are removed on the source.

For objects updated on the source to also update on the target, use –overwrite.
To remove objects from the target that are not on the source, use –remove.

[`mc mirror --watch`](#mc.mirror.-watch) relies on the [[reference/minio-mc#command-mc|`mc`]] removal API for deleting objects. As part of
removing the last object in a bucket prefix, [[reference/minio-mc#command-mc|`mc`]] also recursively removes
each empty part of the prefix up to the bucket root. [[reference/minio-mc#command-mc|`mc`]] only applies the
recursive removal to prefixes created implicitly as part of object write
operations - that is, the prefix was not created using an explicit directory
creation command such as [[reference/minio-mc/mc-mb#command-mc.mb|`mc mb`]].

For example, consider a bucket `photos` with the following object prefixes:

`photos/2021/january/myphoto.jpg`

`photos/2021/february/myotherphoto.jpg`

`photos/NYE21/NewYears.jpg`

`photos/NYE21` is the only prefix explicitly created using [[reference/minio-mc/mc-mb#command-mc.mb|`mc mb`]].
All other prefixes were implicitly created as part of writing the object
located at that prefix.

If an [[reference/minio-mc#command-mc|`mc`]] command removes `myphoto.jpg`, the removal API automatically
trims the empty `/january` prefix. If a subsequent [[reference/minio-mc#command-mc|`mc`]] command removes
`myotherphoto.jpg`, the removal API automatically trims both the `/february`
prefix and the now-empty `/2021` prefix. If an [[reference/minio-mc#command-mc|`mc`]] command removes
`NewYears.jpg`, the `/NYE21` prefix remains in place since it was
explicitly created.

If using [`mc mirror --watch`](#mc.mirror.-watch) for operations on a filesystem, [[reference/minio-mc#command-mc|`mc`]] applies this same
behavior by recursively trimming empty directory paths up to the root. However,
the [[reference/minio-mc#command-mc|`mc`]] remove API cannot distinguish between an explicitly created
directory path and an implicitly created one. If [`mc mirror --watch`](#mc.mirror.-watch) deletes the last
object at a filesystem path, [[reference/minio-mc#command-mc|`mc`]] recursively deletes all empty directories
within that path up to the root as part of the removal operation.

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