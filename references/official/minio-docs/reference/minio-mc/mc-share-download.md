---
title: "mc share download — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-share-download.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-share-download"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc share download — SILO (MinIO Object Storage)"
---
# mc share download

## Syntax

The [`mc share download`](#command-mc.share.download) command generates a temporary presigned URL with
integrated access credentials for downloading objects from a MinIO bucket. The
temporary URL expires after a configurable time limit.

Applications can perform a `GET` to retrieve the object from the URL.

Users can open the URL in a browser to download the object.

For more information on shareable object URLs, see the Amazon S3
documentation on [Pre-Signed URLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html).

EXAMPLE

The following command generates a new presigned download URL for the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc share download --recursive myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] share download           \
                 [--expire "string"]      \
                 [--recursive]            \
                 [--version-id "string"]  \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.share.download.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deplyment and the full path to
the object for which to generate a download URL. For example:

```
mc share download play/mybucket/object.txt
```

You can specify multiple objects on the same or different MinIO deployments.
For example:

```
mc share download play/mybucket/object.txt play/mybucket/otherobject.txt
```

If specifying the path to a bucket or bucket prefix, you must also
specify the [`--recursive`](#mc.share.download.-recursive) argument. For
example:

```
mc share download --recursive play/mybucket/

mc share download --recursive play/mybucket/myprefix/
```

--expire, E[](#mc.share.download.-expire)
Optional

Set the expiration time limit for all generated URLs.

Specify a string with format `##h##m##s` format. For example:
`12h34m56s` for an expiry of 12 hours, 34 minutes, and 56 seconds
after URL generation.

Defaults to `168h` or 168 hours (7 days).

--recursive, r[](#mc.share.download.-recursive)
Optional

Recursively generate URLs for all objects in a
[`mc share download ALIAS`](#mc.share.download.ALIAS) bucket or bucket prefix.

Required if any `ALIAS` specifies a path to a bucket or bucket prefix.

--version-id, vid[](#mc.share.download.-version-id)
Optional

Directs [`mc share download`](#command-mc.share.download) to operate only on the specified object version.

[`--version-id`](#mc.share.download.-version-id) requires that the specified [`ALIAS`](#mc.share.download.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Generate a URL to Download Object(s)

Get Specific Object

Use [`mc share download`](#command-mc.share.download) to generate a URL that supports
`GET` requests for an object:

```
mc share download --expire DURATION ALIAS/PATH
```

Replace [`ALIAS`](#mc.share.download.ALIAS) with the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

Replace [`PATH`](#mc.share.download.ALIAS) with the path to the
object on the MinIO deployment.

Replace [`DURATION`](#mc.share.download.-expire) with the duration
after which the URL expires. For example, to set a 30 day expiry,
specify `30d`.

Get Object(s) in a Bucket

Use [`mc share download`](#command-mc.share.download) with the
[`--recursive`](#mc.share.download.-recursive) option to generate a URL for
each object in a bucket. Each URL supports `GET` requests for its
associated object:

```
mc share download --recursive --expire DURATION ALIAS/PATH
```

Replace [`ALIAS`](#mc.share.download.ALIAS) with the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

Replace [`PATH`](#mc.share.download.ALIAS) with the path to the
bucket or bucket prefix on the MinIO deployment.

Replace [`DURATION`](#mc.share.download.-expire) with the duration
after which the URL expires. For example, to set a 30 day expiry,
specify `30d`.

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