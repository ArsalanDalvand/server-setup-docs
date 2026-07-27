---
title: "mc pipe — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-pipe.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-pipe"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc pipe — SILO (MinIO Object Storage)"
---
# mc pipe

## Syntax

The [`mc pipe`](#command-mc.pipe) command streams content from [STDIN](https://www.gnu.org/software/libc/manual/html_node/Standard-Streams.html) to a target object.

EXAMPLE

The following command writes contents of `STDIN` to an S3 compatible storage.

```
echo "My Meeting Notes" | mc pipe s3/engineering/meeting-notes.txt
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] pipe                              \
                 TARGET                            \
                 [--attr "string"]                 \
                 [--checksum "string"]             \
                 [--enc-kms "string"]              \
                 [--enc-s3 "string"]               \
                 [--enc-c "string"]                \
                 [--storage-class, --sc "string"]  \
                 [--tags "string"]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

Changed in version RELEASE.2023-01-11T03-14-16Z: `mc pipe` now supports concurrent uploads for better throughput of large streams.

### Parameters

TARGET[](#mc.pipe.TARGET)
Required

The full path to the [[reference/minio-mc/mc-alias-set#minio-mc-alias|alias]] or prefix where the command should run.

--attr[](#mc.pipe.-attr)
Optional

Add custom metadata for the object.

Specify key-value pairs as `KEY=VALUE\;`, separating each pair with a back slash and semicolon (`\;`).
For example, `--attr key1=value1\;key2=value2\;key3=value3`.

--checksum[](#mc.pipe.-checksum)
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

--enc-kms[](#mc.pipe.-enc-kms)

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

--enc-s3[](#mc.pipe.-enc-s3)
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

--enc-c[](#mc.pipe.-enc-c)
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

--storage-class, --sc[](#mc.pipe.-storage-class)
Optional

Set the storage class for the new object at the [`TARGET`](#mc.pipe.TARGET).

See [Amazons documentation](https://docs.aws.amazon.com/AmazonS3/latest/dev/storage-class-intro.html) for more information on S3 storage classes.

--tags[](#mc.pipe.-tags)
Optional

Applies one or more tags to the TARGET.

Specify an ampersand-separated list of key-value pairs as `KEY1=VALUE1&KEY2=VALUE2`, where each pair represents one tag to assign to the objects.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Write Contents of STDIN to the Local Filesystem

The following command writes the contents of STDIN to the `/tmp` folder on the local filesystem.

```
mc pipe /tmp/hello-world.go
```

### Copy an ISO Image to S3 Storage

The following command first streams the contents of an iso image for Debian and then uses the stream to create the object at an S3 path.

```
cat debian-live-11.5.0-amd64-mate.iso | mc pipe s3/opensource-isos/debian-11-5.iso
```

### Stream MySQL Database Dump to S3

The following command first streams a MySQL database and uses the stream to create a backup on S3 with [`mc pipe`](#command-mc.pipe):

```
mysqldump -u root -p ******* accountsdb | mc pipe s3/sql-backups/backups/accountsdb-sep-28-2022.sql
```

### Write a File to a Reduced Redundancy Storage Class

The following command takes the STDIN stream and creates an object on the Reduced Redundancy storage class on S3.

```
mc pipe --storage-class REDUCED_REDUNDANCY s3/personalbuck/meeting-notes.txt
```

### Copy a File to a MinIO Deployment with Metadata

The following command uploads an MP3 file to a MinIO deployment with an ALIAS of `myminio` and a `music` bucket.
The object writes with some metadata for `Cache-Control` and `Artist`.

```
cat music.mp3 | mc pipe --attr "Cache-Control=max-age=90000,min-fresh=9000;Artist=Unknown" myminio/music/guitar.mp3
```

### Set Tags on Uploaded Objects

The following command creates an object on a MinIO deployment with an ALIAS of `myminio` in bucket `mybucket` with two tags.
MinIO supports adding up to 10 custom tags to an object.

```
tar cvf - . | mc pipe --tags "category=prod&type=backup" myminio/mybucket/backup.tar
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).