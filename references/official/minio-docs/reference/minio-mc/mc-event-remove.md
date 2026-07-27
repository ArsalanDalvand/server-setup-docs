---
title: "mc event rm — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-event-remove.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-event-remove"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc event rm — SILO (MinIO Object Storage)"
---
# mc event rm

## Syntax

The [`mc event rm`](#command-mc.event.rm) command removes an event notification trigger from a bucket.

The [`mc event remove`](#command-mc.event.remove) command has equivalent functionality to [`mc event rm`](#command-mc.event.rm).

EXAMPLE

The following command removes a configured event notifications for the
specified [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification target]]
for the `mydata` bucket on the `myminio` MinIO deployment:

```
mc event rm myminio/mydata arn:aws:sqs::primary:target
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] event remove        \
                 ALIAS               \
                 [ARN]               \
                 [--event "string"]  \
                 [--force]           \
                 [--prefix "string"] \
                 [--suffix "string"]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

```
mc [GLOBALFLAGS] event remove [FLAGS] ALIAS ARN
```

### Parameters

ALIAS[](#mc.event.rm.ALIAS)
Required

The S3 service [[reference/minio-mc/mc-alias-set#alias|alias]] and bucket from which the command removes the event notification.
For example:

```
mc event rm play/mybucket
```

ARN[](#mc.event.rm.ARN)
Required

The [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference-arns) of the notification target.

The MinIO server outputs an ARN at startup for each configured notification target.
See [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket notifications]] for more information.

Retrieve the ARN by running [[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]] on the bucket.

--event[](#mc.event.rm.-event)
Optional

The event type(s) specified when the event was added.
The entries must match the values used when adding the event.
If no event matches the list of event types, the command returns a `no notification configuration matched` error.

Specify multiple events using a comma `,` delimiter.
See [[reference/minio-mc/mc-event-add#mc-event-supported-events|Supported Bucket Events]] for supported event types.

Defaults to removing an event that triggers for all event types on the [`ALIAS`](#mc.event.rm.ALIAS) bucket with the [`ARN`](#mc.event.rm.ARN) notification target.

Retrieve the event types used by running [[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]] on the bucket.
Use the following table to convert event types in the command’s output to the entry required for the [`mc event rm`](#command-mc.event.rm) command:

Output of `mv event ls`

Event type to use

`s3:objectAccessed`

`get`

`s3:objectCreated`

`put`

`s3:objectRemoved`

`delete`

For example, if the `mc event ls` returns the following:

```
arn:minio:sqs::mytest:webhook   s3:ObjectAccessed:*,s3:ObjectCreated:*   Filter:
```

Use the following command to remove the event:

```
mc event rm alias/bucket arn:minio:sqs::mytest:webhook --event get,put
```

The order of event types does not matter, only that you include the same ones that exist for the event.

--force[](#mc.event.rm.-force)
Optional

Removes all events on the [`ALIAS`](#mc.event.rm.ALIAS) bucket with the [`ARN`](#mc.event.rm.ARN) notification target.

--prefix[](#mc.event.rm.-prefix)
Optional

The bucket prefix in which the command removes bucket notifications.

For example, given a [`ALIAS`](#mc.event.rm.ALIAS) of
`play/mybucket` and a [`--prefix`](#mc.event.rm.-prefix) of
`photos`, the command only removes bucket notifications in
`play/mybucket/photos`.

--suffix[](#mc.event.rm.-suffix)
Optional

The bucket suffix in which the command removes bucket notifications.

For example, given a [`ALIAS`](#mc.event.rm.ALIAS) of
`play/mybucket` and a [`--suffix`](#mc.event.rm.-suffix) of
`.jpg`, the command only removes bucket notifications in
`play/mybucket/*.jpg`.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Remove Event Notifications from a Bucket

Example

The following command removes all event notification triggers on a bucket.
The command assumes the MinIO deployment has at least one configured
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification target]]:

```
mc event rm myminio/mydata arn:minio:sqs::primary:webhook
```

Syntax

```
mc event rm ALIAS ARN
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