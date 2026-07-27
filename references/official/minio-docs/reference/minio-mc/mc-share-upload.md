---
title: "mc share upload — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-share-upload.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-share-upload"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc share upload — SILO (MinIO Object Storage)"
---
# mc share upload

## Syntax

The [`mc share upload`](#command-mc.share.upload) command generates a temporary presigned URL with
integrated access credentials for uploading objects to a MinIO bucket. The
temporary URL expires after a configurable time limit.

Applications can perform a `PUT` to upload an object using the URL.

For more information on shareable object URLs, see the Amazon S3
documentation on [Pre-Signed URLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html).

EXAMPLE

The following command generates a new presigned upload URL for the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc share upload --recursive myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] share upload               \
                 [--content-type "string"]  \
                 [--expire "string"]        \
                 [--recursive]              \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.share.upload.ALIAS)

Required The [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deplyment and the full path to
the object for which to generate an upload URL. For example:

```
mc share upload play/mybucket/object.txt
```

You can specify multiple objects on the same or different MinIO deployments.
For example:

```
mc share upload play/mybucket/object.txt play/mybucket/otherobject.txt
```

If specifying the path to a bucket or bucket prefix, you must also
specify the [`--recursive`](#mc.share.upload.-recursive) argument. For
example:

```
mc share upload --recursive play/mybucket/

mc share upload --recursive play/mybucket/myprefix/
```

--content-type, T[](#mc.share.upload.-content-type)

Optional Restrict uploads to only requests with a specific [Content-Type](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html) header.

Specify a string with the desired `Content-Type` value to accept.
For example, `video/mp4`.

If configured, clients using the generated URL must include a `Content-Type` header for the specified type.
MinIO rejects requests that do not have the correct `Content-Type` header.

Content types are also known as [media types](https://www.iana.org/assignments/media-types/media-types.xhtml).

--expire, E[](#mc.share.upload.-expire)

Optional Set the expiration time limit for all generated URLs.

Specify a string with format `##h##m##s` format. For example:
`12h34m56s` for an expiry of 12 hours, 34 minutes, and 56 seconds
after URL generation.

Defaults to `168h` or 168 hours (7 days).

--recursive, r[](#mc.share.upload.-recursive)

Optional Modifies the CURL URL to support uploading objects to a bucket or
bucket prefix. Required if any `ALIAS` specifies a path to a bucket or
bucket prefix. The modified CURL output resembles the following:

```
curl ... -F key=<NAME> -F file=@<FILE>
```

Replace `<FILE>` with the path to the file to upload.

Replace `<NAME>` with the object name once uploaded.
This may include [[glossary#term-prefix|prefixes]].

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Generate a URL to Upload Object(s)

Upload Single Object

Use [`mc share upload`](#command-mc.share.upload) to generate a URL that supports
`POST` requests for uploading a file to a specific object location
on a MinIO deployment:

```
mc share upload --expire DURATION ALIAS/PATH
```

Replace [`ALIAS`](#mc.share.upload.ALIAS) with the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

Replace [`PATH`](#mc.share.upload.ALIAS) with the path to the
object on the MinIO deployment.

Replace [`DURATION`](#mc.share.upload.-expire) with the duration
after which the URL expires. For example, to set a 30 day expiry,
specify `30d`.

Upload Multiple Objects

Use [`mc share upload`](#command-mc.share.upload) with the
[`--recursive`](#mc.share.upload.-recursive) and
[`--expire`](#mc.share.upload.-expire) options to generate a temporary URL
that supports `POST` requests for uploading files to a bucket on a MinIO
deployment:

```
mc share upload --recursive --expire DURATION ALIAS/PATH
```

Replace [`ALIAS`](#mc.share.upload.ALIAS) with the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

Replace [`PATH`](#mc.share.upload.ALIAS) with the path to the
bucket or bucket prefix on the MinIO deployment.

Replace [`DURATION`](#mc.share.upload.-expire) with the duration
after which the URL expires. For example, to set a 30 day expiry,
specify `30d`.

The command returns a CURL command for uploading an object to the
specified bucket prefix.

Replace the `<FILE>` string in the returned CURL command with the path
to the file to upload.

Replace the `<NAME>` string in the returned CURL command with the name
of the object in the bucket.
This may include [[glossary#term-prefix|prefixes]].

You can use a shell script loop to recursively upload the contents of a
filesystem directory to the S3-compatible service:

```
#!/bin/sh

for file in ~/Documents/photos/
do
   curl https://play.min.io/mybucket/ \
   -F policy=AAAAA -F x-amz-algorithm=AWS4-HMAC-SHA256 \
   -F x-amz-credential=AAAA/us-east-1/s3/aws4_request \
   -F x-amz-date=20200812T202556Z \
   -F x-amz-signature=AAAA \
   -F bucket=mybucket -F key=photos/${file} -F file=@${file}

done
```

This example will upload each file in the directory `~/Documents/photos/` to
the `mybucket` bucket under the prefix `photos`.  Defer to the documented
best practices for your preferred scripting language for iterating through
files in a directory.

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