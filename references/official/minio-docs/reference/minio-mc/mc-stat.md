---
title: "mc stat — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-stat.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-stat"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc stat — SILO (MinIO Object Storage)"
---
# mc stat

## Syntax

The [`mc stat`](#command-mc.stat) command displays information on objects in a MinIO bucket, including object metadata.
You can also use it to retrieve bucket metadata.

You can use [`mc stat`](#command-mc.stat) against the local filesystem to produce similar results to the `stat` commandline tool.

EXAMPLE

The following command displays information on all objects in the `mydata` bucket on the `myminio` MinIO deployment:

```
mc stat --recursive myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] stat                      \
                 [--enc-c "value"]         \
                 [--no-list]               \
                 [--recursive]             \
                 [--rewind "string"]       \
                 [--versions]              \
                 [--version-id "string"]*  \
                 ALIAS [ALIAS ...]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

[`mc stat --version-id`](#mc.stat.-version-id) is mutually exclusive with multiple parameters. See the reference documentation for more information.

### Parameters

ALIAS[](#mc.stat.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment and the full path to the object for which to retrieve detailed information. For example:

```
mc stat myminio/mybucket/myobject.txt
```

You can specify multiple objects on the same or different MinIO deployments:

```
mc stat myminio/mybucket/myobject.txt myminio/mybucket/myobject.txt
```

If specifying the path to a bucket or bucket prefix, you must include the [`mc stat --recursive`](#mc.stat.-recursive) flag:

```
mc stat --recursive myminio/mybucket/
```

For retrieving information on a file from a local filesystem, specify the full path to that file:

```
mc stat ~/data/myobject.txt
```

--enc-c[](#mc.stat.-enc-c)
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

--no-list[](#mc.stat.-no-list)
Optional

Disable all `LIST` operations if the target does not exist.

--recursive, r[](#mc.stat.-recursive)
Optional

Recursively [`mc stat`](#command-mc.stat) the contents of the MinIO bucket specified to [`ALIAS`](#mc.stat.ALIAS).

--rewind[](#mc.stat.-rewind)
Optional

Directs [`mc stat`](#command-mc.stat) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.stat.-rewind) requires that the specified [`ALIAS`](#mc.stat.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

--versions[](#mc.stat.-versions)
Optional

Directs [`mc stat`](#command-mc.stat) to operate on all object versions that exist in the
bucket.

[`--versions`](#mc.stat.-versions) requires that the specified [`ALIAS`](#mc.stat.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Use [`--versions`](#mc.stat.-versions) and  [`--rewind`](#mc.stat.-rewind) together to remove all object versions which existed at a specific point in time.

--version-id, vid[](#mc.stat.-version-id)
Optional

Directs [`mc stat`](#command-mc.stat) to operate only on the specified object version.

[`--version-id`](#mc.stat.-version-id) requires that the specified [`ALIAS`](#mc.stat.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Mutually exclusive with any of the following flags:

[`--versions`](#mc.stat.-versions)

[`--rewind`](#mc.stat.-rewind)

[`--recursive`](#mc.stat.-recursive)

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Display Object Details

The following example displays details of the object `myfile.txt` in the bucket `mybucket`:

```
mc stat myminio/mybucket/myfile.txt
```

The output resembles the following:

```
Name      : myfile.txt
Date      : 2024-07-16 15:40:02 MDT
Size      : 6.0 KiB
ETag      : 3b38f7b05a0c42acdc377e60b2a74ddf
Type      : file
Metadata  :
  Content-Type: text/plain
```

You can specify more than one object by adding multiple paths:

```
mc stat myminio/mybucket/file1.txt myminio/yourbucket/file2.txt
```

To display detail for all objects in a bucket, use [`--recursive`](#mc.stat.-recursive).
The following example displays details for all objects in bucket `mybucket`:

```
mc stat --recursive myminio/mybucket
```

The output resembles the following:

```
Name      : file1.txt
Date      : 2024-07-16 15:40:02 MDT
Size      : 6.0 KiB
ETag      : 3b38f7b05a0c42acdc377e60b2a74ddf
Type      : file
Metadata  :
  Content-Type: text/plain

Name      : file2.txt
Date      : 2024-07-26 10:45:19 MDT
Size      : 6.0 KiB
ETag      : 3b38f7b05a0c42acdc377e60b2a74ddf
Type      : file
Metadata  :
  Content-Type: text/plain
```

### Display Bucket Details

The following example displays information about the bucket `mybucket` on the `myminio` MinIO deployment:

```
mc stat myminio/mybucket
```

The output resembles the following:

```
Name      : mybucket
Date      : 2024-07-26 10:56:43 MDT
Size      : N/A
Type      : folder

Properties:
  Versioning: Un-versioned
  Location: us-east-1
  Anonymous: Disabled
  ILM: Disabled

Usage:
      Total size: 6.0 KiB
   Objects count: 1
  Versions count: 0

Object sizes histogram:
   1 object(s) BETWEEN_1024B_AND_1_MB
   1 object(s) BETWEEN_1024_B_AND_64_KB
   0 object(s) BETWEEN_10_MB_AND_64_MB
   0 object(s) BETWEEN_128_MB_AND_512_MB
   0 object(s) BETWEEN_1_MB_AND_10_MB
   0 object(s) BETWEEN_256_KB_AND_512_KB
   0 object(s) BETWEEN_512_KB_AND_1_MB
   0 object(s) BETWEEN_64_KB_AND_256_KB
   0 object(s) BETWEEN_64_MB_AND_128_MB
   0 object(s) GREATER_THAN_512_MB
   0 object(s) LESS_THAN_1024_B
```

### Count of Objects in a Bucket

To show the number of objects in a bucket, use `--json` and extract the value of `objectsCount` with a JSON parser:

The following example uses the [jq](https://jqlang.github.io/jq/) utility:

```
mc stat myminio/mybucket --json | jq '.Usage.objectsCount'
```

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