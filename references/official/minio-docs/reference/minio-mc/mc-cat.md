---
title: "mc cat — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-cat.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-cat"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc cat — SILO (MinIO Object Storage)"
---
# mc cat

## Syntax

The [`mc cat`](#command-mc.cat) command concatenates the contents of a file or
object to another file or object. You can also use the command to
display the contents of the specified file or object to `STDOUT`.
[`cat`](#command-mc.cat) has similar functionality to `cat`.

EXAMPLE

The following command concatenates the contents of an object on a
MinIO deployment to `STDOUT`:

```
mc cat play/mybucket/myobject.txt
```

SYNTAX

The [`mc cat`](#command-mc.cat) command has the following syntax:

```
mc [GLOBALFLAGS] cat                       \
                 ALIAS [ALIAS ...]         \
                 [--enc-c "value"]         \
                 [--offset "int"]          \
                 [--part-number "int"]     \
                 [--rewind]                \
                 [--tail "int"]            \
                 [--version-id "string"]   \
                 [--zip]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

You can also use [`mc cat`](#command-mc.cat) against a local filesystem to produce similar results to the `cat` commandline tool.

### Parameters

ALIAS[](#mc.cat.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment and the full
path to the object. For example:

```
mc cat myminio/mybucket/myobject.txt
```

You can specify multiple objects on the same or different MinIO
deployment. For example:

```
mc cat myminio/mybucket/object.txt myminio/myotherbucket/object.txt
```

For an object on a local filesystem, specify the full path to that
object. For example:

```
mc cat ~/data/object.txt
```

--enc-c[](#mc.cat.-enc-c)
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

--offset[](#mc.cat.-offset)
Optional

Specify an integer that is the number of bytes from which the command offsets the output.

Mutually exclusive with the [`--part-number`](#mc.cat.-part-number) flag.

--part-number[](#mc.cat.-part-number)
Optional

Download a specific part number of a multi-part upload.
Specify the integer of the part number to download.

Mutually exclusive with the [`--offset`](#mc.cat.-offset) and [`--tail`](#mc.cat.-tail) flags.

--rewind[](#mc.cat.-rewind)
Optional

Directs [`mc cat`](#command-mc.cat) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.cat.-rewind) requires that the specified [`ALIAS`](#mc.cat.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

--tail[](#mc.cat.-tail)
Optional

Specify an integer that is the number of bytes from which the command trims the output.

Mutually exclusive with the [`--part-number`](#mc.cat.-part-number) flag.

--version-id, vid[](#mc.cat.-version-id)
Optional

Directs [`mc cat`](#command-mc.cat) to operate only on the specified object version.

[`--version-id`](#mc.cat.-version-id) requires that the specified [`ALIAS`](#mc.cat.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

--zip[](#mc.cat.-zip)
Optional

Extracts the contents from a zip file on the source to the remote.
Requires a MinIO deployment as the source `ALIAS`.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### View an S3 Object

Use [`mc cat`](#command-mc.cat) to return the object:

```
mc cat ALIAS/PATH
```

Replace [`ALIAS`](#mc.cat.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.cat.ALIAS) with the path to the object on the
S3-compatible host.

### View an S3 Object at a Point-In-Time

Use [`mc cat --rewind`](#mc.cat.-rewind) to return the object at a specific
point-in-time in the past:

```
mc cat ALIAS/PATH --rewind DURATION
```

Replace [`ALIAS`](#mc.cat.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.cat.ALIAS) with the path to the object on the
S3-compatible host.

Replace [`DURATION`](#mc.cat.-rewind) with the point-in-time in the past
at which the command returns the object. For example, specify `30d` to
return the version of the object 30 days prior to the current date.

Requires Versioning

[`mc cat`](#command-mc.cat) requires [[administration/object-management/object-versioning#minio-bucket-versioning|bucket versioning]] to
use this feature. Use [[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable versioning on a bucket.

### View an S3 Object with Specific Version

Use [`mc cat --version-id`](#mc.cat.-version-id) to return a specific version of the
object:

```
mc cat ALIAS/PATH --version-id VERSION
```

Replace [`ALIAS`](#mc.cat.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.cat.ALIAS) with the path to the object on the
S3-compatible host.

Replace [`VERSION`](#mc.cat.-version-id) with the specific version of the
object to return.

Requires Versioning

[`mc cat`](#command-mc.cat) requires [[administration/object-management/object-versioning#minio-bucket-versioning|bucket versioning]] to
use this feature. Use [[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable versioning on a bucket.

### Download a particular part

Use [`mc cat --part-number`](#mc.cat.-part-number) to download a particular part of a multi-part upload:

```
mc cat ALIAS/PATH --part-number=#
```

Replace [`ALIAS`](#mc.cat.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.cat.ALIAS) with the path to the object on the S3-compatible host.

Replace `#` with the integer of the part number to download.
For example, to download part 3 of at 16-part multi-part file, use `--part-number=3`.

You cannot use the `--part-number` flag if you are using either the `--offset` or the `--tail` flags.

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