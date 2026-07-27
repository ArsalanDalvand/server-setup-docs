---
title: "mc event ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-event-list.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-event-list"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc event ls — SILO (MinIO Object Storage)"
---
# mc event ls

## Syntax

The [`mc event ls`](#command-mc.event.ls) command lists all event notification triggers for a
bucket.

The alias [`mc event list`](#command-mc.event.list) has equivalent functionality to [`mc event ls`](#command-mc.event.ls).

EXAMPLE

The following command lists all configured event notifications for the
specified [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification target]]
for the `mydata` bucket on the `myminio` MinIO deployment:

```
mc event ls myminio myminio/mydata arn:aws:sqs::primary:target
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

```
mc [GLOBALFLAGS] event ls [FLAGS] ALIAS ARN
```

### Parameters

ALIAS[](#mc.event.ls.ALIAS)
Required

The S3 service [[reference/minio-mc/mc-alias-set#alias|alias]] and bucket to which the command lists event notification.
For example:

```
mc event ls play/mybucket ARN...
```

ARN[](#mc.event.ls.ARN)
Required

The [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference-arns) of the bucket resource.

The MinIO server outputs an ARN at startup for each configured notification target.
See [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]] for more information.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### List Event Notifications on a Bucket

Example

The following command lists all event notification triggers on a bucket.

```
mc event ls myminio/mydata
```

Syntax

```
mc event ls ALIAS ARN
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO
deployment on which to add the bucket notification event. For example:

`myminio/mydata`

Replace `ARN` with the notification target
[[reference/minio-mc/mc-event-add#mc.event.add.ARN|`ARN`]].

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