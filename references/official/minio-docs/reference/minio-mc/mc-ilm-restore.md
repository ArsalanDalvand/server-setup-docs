---
title: "mc ilm restore — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm-restore.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm-restore"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm restore — SILO (MinIO Object Storage)"
---
# mc ilm restore

## Syntax

The [`mc ilm restore`](#command-mc.ilm.restore) command creates a temporary copy of an object archived
on a remote tier. The copy automatically expires after 1 day by default.

Use this command to allow applications to access a tiered object through the
MinIO deployment (e.g. “hot tier”). The archived object remains on the remote
tier, while the temporary copy becomes `HEAD` for that object.

New in version mc: RELEASE.2023-04-12T02-21-51Z

Use [[reference/minio-mc/mc-stat#command-mc.stat|`mc stat`]] to display whether a restored object reads from the local temporary copy or the remote tier.
Objects currently in the process of restoration from the remote tier show a status of `Ongoing : true`.

EXAMPLE

The following command restores a copy of a transitioned object from the
remote tier back to the `myminio` MinIO deployment:

```
mc ilm restore myminio/mybucket/object.txt
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] ilm restore         \
                 [--days "int" ]     \
                 [--recursive]       \
                 [--vid "string"]    \
                 [--versions]        \
                 [--enc-c "string"]  \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.ilm.restore.ALIAS)
Required

The MinIO [[reference/minio-mc/mc-alias-set#alias|alias]], bucket, and path to the archived object to restore.

```
mc ilm restore myminio/mybucket/object.txt
```

--days[](#mc.ilm.restore.-days)
Optional

The number of days after which MinIO expires the restored copy of the archived object.

--enc-c[](#mc.ilm.restore.-enc-c)
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

--recursive, r[](#mc.ilm.restore.-recursive)
Optional

Restores all objects under the specified prefix.

--versions[](#mc.ilm.restore.-versions)
Optional

Restores all versions of the object on the remote tier.

--version-id, vid[](#mc.ilm.restore.-version-id)
Optional

Restores the specified version of the object on the remote tier.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Restore an Archived Object

The following command restores an object archived to a remote tier:

```
mc ilm restore myminio/mybucket/object.txt
```

### Restore a Specific Archived Object Version

The following command restore a specific object version archived to a
remote tier:

```
mc ilm restore --vid "VERSIONID" myminio/mybucket/object.txt
```

### Restore All Archived Objects at a Bucket Prefix

The following command restores all objects archived under a specified prefix on
the remote tier:

```
mc ilm restore --recursive myminio/mybucket/data/
```

## Behavior

### Restored Objects Expire Automatically

MinIO automatically expires the restored object copy after the specified
number of days (Default: 1 day).

### Restored Objects Become HEAD

The restored object copy becomes HEAD for that object namespace regardless
of it’s versioning history. This can result in applications returning
“stale” data while the local copy exists.

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