---
title: "mc event add — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-event-add.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-event-add"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc event add — SILO (MinIO Object Storage)"
---
# mc event add

## Syntax

The [`mc event add`](#command-mc.event.add) command adds event notification triggers to a bucket.

MinIO automatically sends triggered events to the configured
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|notification target]].

EXAMPLE

The following command creates a new event notification trigger for
all `PUT` and `DELETE` operations for the `mydata` bucket on the
`myminio` MinIO deployment:

```
mc event add --event "put,delete" myminio/mydata arn:aws:sqs::primary:target
```

The specified ARN corresponds to a configured
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification target]] on the
`myminio` deployment.

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] event add \
                 [--event "string"]  \
                 [--ignore-existing] \
                 [--prefix "string"] \
                 [--suffix "string"] \
                 ALIAS               \
                 ARN
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.event.add.ALIAS)
Required

The MinIO [[reference/minio-mc/mc-alias-set#alias|alias]] and bucket to which the command adds the new event notification.
For example:

```
mc event add play/mybucket
```

ARN[](#mc.event.add.ARN)
Required

The [Amazon Resource Name (ARN)](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference-arns) of the notification target.

The MinIO server outputs an ARN for each configured notification target at server startup.
See [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]] for more information.

--event[](#mc.event.add.-event)
Optional

The event(s) for which MinIO generates bucket notifications.

Supports the following values:

`put`

`get`

`delete`

Specify multiple value using a comma `,` delimiter.
Do not add any whitespace between values.

If not specified, defaults to `put,delete,get`.

See [Supported Bucket Events](#mc-event-supported-events) for a detailed list of S3 events associated to each of the supported values.

ignore-existing, p[](#mc.event.add.ignore-existing)
Optional

Directs MinIO to ignore the specified event triggers if a matching trigger already exists.

--prefix[](#mc.event.add.-prefix)
Optional

The bucket prefix in which the specified [`--event`](#mc.event.add.-event) can trigger a bucket notification.

For example, given a [`ALIAS`](#mc.event.add.ALIAS) of `play/mybucket` and a [`--prefix`](#mc.event.add.-prefix) of `photos`, only events in `play/mybucket/photos` trigger bucket notifications.

Omit to trigger the event for all prefixes and objects in the bucket.

--suffix[](#mc.event.add.-suffix)
Optional

The bucket suffix in which the specified [`--event`](#mc.event.add.-event) can trigger a bucket notification.

For example, given a [`ALIAS`](#mc.event.add.ALIAS) of `play/mybucket` and a [`--suffix`](#mc.event.add.-suffix) of `.jpg`, only events in `play/mybucket/*.jpg` trigger bucket notifications.

Omit to trigger the event for all objects regardless of suffix.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Add an Event Notification to a Bucket

Example

The following command adds a new event notification trigger for all
S3 `PUT`, `GET`, and `DELETE` operations on a bucket. The command
assumes the MinIO deployment has at least one configured
[[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notification target]]:

```
mc event add myminio/mydata arn:minio:sqs::primary:webhook
```

Syntax

```
mc event add ALIAS ARN
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO
deployment and the bucket on which to add the bucket notification event.
For example:

`myminio/mydata`

Replace `ARN` with the notification target
[`ARN`](#mc.event.add.ARN).

## Behavior

### Supported Bucket Events

The following table lists the supported [`mc event add`](#command-mc.event.add) values and their
corresponding [[administration/monitoring/bucket-notifications#minio-bucket-notifications-event-types|S3 events]]:

Supported Value

Corresponding S3 Events

`put`

[[administration/monitoring/bucket-notifications#data.s3-ObjectCreated-CompleteMultipartUpload|`s3:ObjectCreated:CompleteMultipartUpload`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectCreated-Copy|`s3:ObjectCreated:Copy`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectCreated-DeleteTagging|`s3:ObjectCreated:DeleteTagging`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectCreated-Post|`s3:ObjectCreated:Post`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectCreated-Put|`s3:ObjectCreated:Put`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectCreated-PutLegalHold|`s3:ObjectCreated:PutLegalHold`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectCreated-PutRetention|`s3:ObjectCreated:PutRetention`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectCreated-PutTagging|`s3:ObjectCreated:PutTagging`]]

`get`

[[administration/monitoring/bucket-notifications#data.s3-ObjectAccessed-Head|`s3:ObjectAccessed:Head`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectAccessed-Get|`s3:ObjectAccessed:Get`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectAccessed-GetRetention|`s3:ObjectAccessed:GetRetention`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectAccessed-GetLegalHold|`s3:ObjectAccessed:GetLegalHold`]]

`delete`

[[administration/monitoring/bucket-notifications#data.s3-ObjectRemoved-Delete|`s3:ObjectRemoved:Delete`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectRemoved-DeleteMarkerCreated|`s3:ObjectRemoved:DeleteMarkerCreated`]]

`replica`

[[administration/monitoring/bucket-notifications#data.s3-Replication-OperationCompletedReplication|`s3:Replication:OperationCompletedReplication`]]
[[administration/monitoring/bucket-notifications#data.s3-Replication-OperationFailedReplication|`s3:Replication:OperationFailedReplication`]]
[[administration/monitoring/bucket-notifications#data.s3-Replication-OperationMissedThreshold|`s3:Replication:OperationMissedThreshold`]]
[[administration/monitoring/bucket-notifications#data.s3-Replication-OperationNotTracked|`s3:Replication:OperationNotTracked`]]
[[administration/monitoring/bucket-notifications#data.s3-Replication-OperationReplicatedAfterThreshold|`s3:Replication:OperationReplicatedAfterThreshold`]]

`ilm`

[[administration/monitoring/bucket-notifications#data.s3-ObjectTransition-Failed|`s3:ObjectTransition:Failed`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectTransition-Complete|`s3:ObjectTransition:Complete`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectRestore-Post|`s3:ObjectRestore:Post`]]
[[administration/monitoring/bucket-notifications#data.s3-ObjectRestore-Completed|`s3:ObjectRestore:Completed`]]

`scanner`

[[administration/monitoring/bucket-notifications#data.s3-Scanner-ManyVersions|`s3:Scanner:ManyVersions`]]
[[administration/monitoring/bucket-notifications#data.s3-Scanner-BigPrefix|`s3:Scanner:BigPrefix`]]

For more complete documentation on the listed S3 events, see [S3 Supported Event Types](https://docs.aws.amazon.com/AmazonS3/latest/userguide/NotificationHowTo.html#notification-how-to-event-types-and-destinations).

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