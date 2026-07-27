---
title: "mc replicate add — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-replicate-add.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-replicate-add"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc replicate add — SILO (MinIO Object Storage)"
---
# mc replicate add

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc replicate add` replaces the `mc admin bucket remote add` command.

MinIO automatically creates remote targets based on a given file path or resource location (such as an IP or DNS address).
Users defining a remote target no longer need to determine an ARN for the remote bucket.

## Syntax

The [`mc replicate add`](#command-mc.replicate.add) command creates a new [[administration/bucket-replication#minio-bucket-replication-serverside|server-side replication]] rule for a bucket on a MinIO deployment.

The remote bucket must be on a MinIO deployment running the same version of MinIO as the local deployment.

Note

Where [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]] only synchronizes the current version of an object, `mc replicate` synchronizes all versions, version information, and metadata for the objects.

The MinIO deployment automatically begins synchronizing new objects to the remote MinIO deployment after creating the rule.
You can optionally configure synchronization of existing objects, delete operations, and fully-deleted objects.

EXAMPLE

The following command adds a new replication rule for the `mydata` bucket on the `myminio` MinIO deployment:

```
mc replicate add                                                     \
   --remote-bucket https://user:[email protected]:9001/bucket  \
   --replicate "delete,delete-marker,existing-objects"               \
   myminio/mydata
```

The replication rule synchronizes versioned delete operations, delete markers, and existing objects to the remote MinIO deployment.

Changed in version mc: RELEASE.2024-03-03T00-13-08Z

You can use a configured ALIAS to the `--remote-bucket` flag.

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] replicate add                     \
                 --remote-bucket string          \
                 [--bandwidth "string"]            \
                 [--disable]                       \
                 [--disable-proxy]                 \
                 [--healthcheck-seconds integer]   \
                 [--id "string"]                   \
                 [--limit-upload "string"]         \
                 [--limit-download "string"]       \
                 [--path "string"]                 \
                 [--region "string"]               \
                 [--replicate "string"]            \
                 [--storage-class "string"]        \
                 [--sync]                          \
                 [--tags "string"]                 \
                 [--priority int]                  \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.replicate.add.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment and full path to the bucket or bucket prefix on which to create the replication rule.
For example:

```
mc replicate add --remote-bucket https://user:[email protected]:9001/bucket play/mybucket
```

--remote-bucket[](#mc.replicate.add.-remote-bucket)
Required

Changed in version mc: RELEASE.2024-03-03T00-13-08Z

The `--remote-bucket` supports specifying an existing [[reference/minio-mc/mc-alias-set#alias|alias]].

Specify the credentials, destination deployment, and bucket of the remote location.
Value may be an IP address, URL, or [[reference/minio-mc/mc-alias-set#alias|alias]]/bucket.

For example, a URL based target might look like the following:

```
https://user:secret@myminio.cloudprovider.tld:9001/bucket
```

An alias based target might look like the following:

```
--remote-bucket minio-target/my-bucket
```

--bandwidth[](#mc.replicate.add.-bandwidth)
Optional

Limit bandwidth rates to no more than the specified rate in KiB/s, MiB/s, or GiB/s.
Valid units include:

`B` for bytes

`K` for kilobytes

`G` for gigabytes

`T` for terabytes

`Ki` for kibibytes

`Gi` for gibibytes

`Ti` for tebibytes

For example, to limit bandwidth rates to no more than 1 GiB/s, use the following:

```
--limit-upload 1Gi
```

If not specified, MinIO does not limit the bandwidth rate.

--disable[](#mc.replicate.add.-disable)
Optional

Creates the replication rule in the “disabled” state.
MinIO does not begin replicating objects using the rule until it is enabled using [[reference/minio-mc/mc-replicate-update#command-mc.replicate.update|`mc replicate update`]].

Objects created while replication is disabled are not immediately eligible for replication after enabling the rule.
You must explicitly enable replication of existing objects by including `"existing-objects"` to the list of replication features specified to [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-replicate|`mc replicate update --replicate`]].
See [[administration/bucket-replication#minio-replication-behavior-existing-objects|Replication of Existing Objects]] for more information.

--disable-proxy[](#mc.replicate.add.-disable-proxy)
Optional

When defining active-active replication between buckets, do not proxy.

By default, MinIO proxies.

--healthcheck-seconds[](#mc.replicate.add.-healthcheck-seconds)
Optional

The length of time in seconds between checks on the health of the remote bucket.

If not specified, MinIO uses an interval of 60 seconds.

--id[](#mc.replicate.add.-id)
Optional

Specify a unique ID for the replication rule.
MinIO automatically generates an ID if one is not specified.

--limit-download[](#mc.replicate.add.-limit-download)
Optional

Limit download rates to no more than a specified rate in KiB/s, MiB/s, or GiB/s.
Valid units include:

`B` for bytes

`K` for kilobytes

`G` for gigabytes

`T` for terabytes

`Ki` for kibibytes

`Gi` for gibibytes

`Ti` for tebibytes

For example, to limit download rates to no more than 1 GiB/s, use the following:

```
--limit-download 1G
```

If not specified, MinIO uses an unlimited download rate.

--limit-upload[](#mc.replicate.add.-limit-upload)
Optional

Limit upload rates to no more than the specified rate in KiB/s, MiB/s, or GiB/s.
Valid units include:

`B` for bytes

`K` for kilobytes

`G` for gigabytes

`T` for terabytes

`Ki` for kibibytes

`Gi` for gibibytes

`Ti` for tebibytes

For example, to limit upload rates to no more than 1 GiB/s, use the following:

```
--limit-upload 1G
```

If not specified, MinIO uses an unlimited upload rate.

--path[](#mc.replicate.add.-path)
Optional

Enable path-style lookup support for the remote bucket.

Valid values include:

`on` - use a path lookup to find the remote bucket

`off` - use a resource locator style (such as a domain or IP address) lookup to find the remote bucket

`auto` - ask MinIO to identify the correct type of lookup to use to find the remote bucket

When not defined, MinIO uses the `auto` value.

--priority[](#mc.replicate.add.-priority)
Optional

Specify the integer priority of the replication rule.
The value must be unique among all other rules on the source bucket.
Higher values imply a higher priority than all other rules.

The default value is `0`.

--region[](#mc.replicate.add.-region)
Optional

The region of the destination bucket to replicate contents to.

--replicate[](#mc.replicate.add.-replicate)
Optional

Specify a comma-separated list of the following values to enable extended replication features.

`delete` - Directs MinIO to replicate [[administration/object-management/object-delete#minio-object-delete|DELETE operations]] to the destination bucket.

`delete-marker` - Directs MinIO to replicate delete markers to the destination bucket.

`existing-objects` - Directs MinIO to replicate objects created before replication was enabled or while replication was suspended.

`metadata-sync` - Directs MinIO to replicate metadata for each object.
For active-active replication situations only.

Omitting this value directs MinIO to stop replicating metadata-only changes back to the source.

If not specified, MinIO syncs all options.

--storage-class[](#mc.replicate.add.-storage-class)
Optional

Specify the MinIO [[reference/minio-server/settings/storage-class#minio-ec-storage-class|storage class]] to apply to replicated objects.

--sync[](#mc.replicate.add.-sync)
Optional

Enable synchronous replication for this remote target.

By default, MinIO uses asynchronous replication.

--tags[](#mc.replicate.add.-tags)
Optional

Specify one or more ampersand `&` separated key-value pair tags which MinIO uses for filtering objects to replicate.
For example:

```
mc replicate add --tags "TAG1=VALUE&TAG2=VALUE&TAG3=VALUE" ALIAS
```

MinIO applies the replication rule to any object whose tag set
contains the specified replication tags.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Configure Bucket Replication

The following [`mc replicate add`](#command-mc.replicate.add) command creates a replication configuration that synchronizes all new objects, existing objects, delete operations, and delete markers to the remote target:

```
mc replicate add myminio/mybucket \
   --remote-bucket https://user:[email protected]/remotebucket \
   --replicate "delete,delete-marker,existing-objects"
```

Replace `myminio/mybucket` with the [`ALIAS`](#mc.replicate.add.ALIAS) and full bucket path for which to create the replication configuration.

Replace the [`--remote-bucket`](#mc.replicate.add.-remote-bucket) value with the URL or path of the remote target.
If using a file path format location, use the `--path on` option.

The [`--replicate`](#mc.replicate.add.-replicate) flag directs MinIO to replicate all delete operations, delete markers, and existing objects to the remote.
See [[administration/bucket-replication#minio-replication-behavior-delete|Replication of Delete Operations]] and [[administration/bucket-replication#minio-replication-behavior-existing-objects|Replication of Existing Objects]] for more information on replication behavior.

### Configure Bucket Replication for Historical Data Record

The following [`mc replicate add`](#command-mc.replicate.add) command creates a new bucket replication configuration that synchronizes all new and existing objects to the remote target:

```
mc replicate add myminio/mybucket \
   --remote-bucket https://user:[email protected]/remotebucket \
   --replicate "existing-objects"
```

Replace `myminio/mybucket` with the [`ALIAS`](#mc.replicate.add.ALIAS) and full bucket path for which to create the replication configuration.

Replace the [`--remote-bucket`](#mc.replicate.add.-remote-bucket) value with the location of the remote target.
If using a file path format location, use the `--path on` option.

The [`--replicate`](#mc.replicate.add.-replicate) flag directs MinIO to replicate all existing objects to the remote.
See [[administration/bucket-replication#minio-replication-behavior-existing-objects|Replication of Existing Objects]] for more information on replication behavior.

The resulting remote copy represents a historical record of objects on the remote, where delete operations on the source have no effect on the remote copy.

## Behavior

### Server-Side Replication Requires MinIO Source and Destination

MinIO server-side replication only works between MinIO deployments.
Both the source and destination deployments must run MinIO.

To configure replication between arbitrary S3-compatible services, use [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]].

### Enable Versioning on Source and Destination Buckets

MinIO relies on the immutability protections provided by versioning to synchronize objects between the source and replication target.

Use the [[reference/minio-mc/mc-version-enable#command-mc.version.enable|`mc version enable`]] command to enable versioning on both the source and destination bucket before starting this procedure:

```
mc version enable ALIAS/PATH
```

Replace [[reference/minio-mc/mc-version-enable#mc.version.enable.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [[reference/minio-mc/mc-version-enable#mc.version.enable.ALIAS|`PATH`]] with the bucket on which to enable versioning.

### Required Permissions

MinIO strongly recommends creating users specifically for supporting bucket replication operations.
See [[reference/minio-mc-admin/mc-admin-user#command-mc.admin.user|`mc admin user`]] and [[reference/minio-mc-admin/mc-admin-policy#command-mc.admin.policy|`mc admin policy`]] for more complete documentation on adding users and policies to a MinIO deployment.

Replication Admin

The following policy provides permissions for configuring and enabling replication on a deployment.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "admin:SetBucketTarget",
                "admin:GetBucketTarget",
                "admin:ListBatchJobs",
                "admin:DescribeBatchJob",
                "admin:StartBatchJob",
                "admin:CancelBatchJob"
            ],
            "Effect": "Allow",
            "Sid": "EnableRemoteBucketConfiguration"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetReplicationConfiguration",
                "s3:ListBucket",
                "s3:ListBucketMultipartUploads",
                "s3:GetBucketLocation",
                "s3:GetBucketVersioning",
                "s3:GetObjectRetention",
                "s3:GetObjectLegalHold",
                "s3:PutReplicationConfiguration"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ],
            "Sid": "EnableReplicationRuleConfiguration"
        }
    ]
}
```

The `"EnableRemoteBucketConfiguration"` statement grants permission for creating a remote target for supporting replication.

The `"EnableReplicationRuleConfiguration"` statement grants permission for creating replication rules on a bucket.
The `"arn:aws:s3:::*` resource applies the replication permissions to any bucket on the source deployment.
You can restrict the user policy to specific buckets as-needed.

Use the [[reference/minio-mc-admin/mc-admin-policy-create#command-mc.admin.policy.create|`mc admin policy create`]] to add this policy to each deployment acting as a replication source.
Use [[reference/minio-mc-admin/mc-admin-user-add#command-mc.admin.user.add|`mc admin user add`]] to create a user on the deployment and [[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`mc admin policy attach`]] to associate the policy to that new user.

Replication Remote User

The following policy provides permissions for enabling synchronization of replicated data into the deployment.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetReplicationConfiguration",
                "s3:ListBucket",
                "s3:ListBucketMultipartUploads",
                "s3:GetBucketLocation",
                "s3:GetBucketVersioning",
                "s3:GetBucketObjectLockConfiguration",
                "s3:GetEncryptionConfiguration"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ],
            "Sid": "EnableReplicationOnBucket"
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:GetReplicationConfiguration",
                "s3:ReplicateTags",
                "s3:AbortMultipartUpload",
                "s3:GetObject",
                "s3:GetObjectVersion",
                "s3:GetObjectVersionTagging",
                "s3:PutObject",
                "s3:PutObjectRetention",
                "s3:PutBucketObjectLockConfiguration",
                "s3:PutObjectLegalHold",
                "s3:DeleteObject",
                "s3:ReplicateObject",
                "s3:ReplicateDelete"
            ],
            "Resource": [
                "arn:aws:s3:::*"
            ],
            "Sid": "EnableReplicatingDataIntoBucket"
        }
    ]
}
```

The `"EnableReplicationOnBucket"` statement grants permission for a remote target to retrieve bucket-level configuration for supporting replication operations on all buckets in the MinIO deployment.
To restrict the policy to specific buckets, specify those buckets as an element in the `Resource` array similar to `"arn:aws:s3:::bucketName"`.

The `"EnableReplicatingDataIntoBucket"` statement grants permission for a remote target to synchronize data into any bucket in the MinIO deployment.
To restrict the policy to specific buckets, specify those buckets as an element in the `Resource` array similar to `"arn:aws:s3:::bucketName/*"`.

Use the [[reference/minio-mc-admin/mc-admin-policy-create#command-mc.admin.policy.create|`mc admin policy create`]] to add this policy to each deployment acting as a replication target.
Use [[reference/minio-mc-admin/mc-admin-user-add#command-mc.admin.user.add|`mc admin user add`]] to create a user on the deployment and [[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`mc admin policy attach`]] to associate the policy to that new user.

### Replication of Existing Objects

Starting with [[reference/minio-mc#command-mc|`mc`]] [RELEASE.2021-06-13T17-48-22Z](https://github.com/minio/mc/releases/tag/RELEASE.2021-06-13T17-48-22Z) and [[reference/minio-server/minio-server#command-minio|`minio`]] [RELEASE.2021-06-07T21-40-51Z](https://github.com/minio/minio/releases/tag/RELEASE.2021-06-07T21-40-51Z), MinIO supports automatically replicating existing objects in a bucket.
MinIO existing object replication implements functionality similar to [AWS Replicating existing objects between S3 buckets](https://aws.amazon.com/blogs/storage/replicating-existing-objects-between-s3-buckets/) without the overhead of contacting technical support.

To enable replication of existing objects when creating a new replication rule, include `"existing-objects"` to the list of replication features specified to [`mc replicate add --replicate`](#mc.replicate.add.-replicate).

To enable replication of existing objects for an existing replication rule, add `"existing-objects"` to the list of existing replication features using [`mc replicate add --replicate`](#mc.replicate.add.-replicate).
You must specify all desired replication features when editing the replication rule.

See [[administration/bucket-replication#minio-replication-behavior-existing-objects|Replication of Existing Objects]] for more complete documentation on this behavior.

### Synchronization of Metadata Changes

MinIO supports [[administration/bucket-replication/enable-server-side-two-way-bucket-replication#minio-bucket-replication-serverside-twoway|two-way active-active]] replication configurations, where MinIO synchronizes new and modified objects between a bucket on two MinIO deployments.
Starting with [[reference/minio-mc#command-mc|`mc`]] [RELEASE.2021-05-18T03-39-44Z](https://github.com/minio/mc/releases/tag/RELEASE.2021-05-18T03-39-44Z), MinIO by default synchronizes metadata-only changes to a replicated object back to the “source” deployment.
Prior to the this update, MinIO did not support synchronizing metadata-only changes to a replicated object.

With metadata synchronization enabled, MinIO resets the object [[administration/bucket-replication#minio-replication-process|replication status]] to indicate replication eligibility.
Specifically, when an application performs a metadata-only update to an object with the `REPLICA` status, MinIO marks the object as `PENDING` and eligible for replication.

To disable metadata synchronization, use the [[reference/minio-mc/mc-replicate-update#mc.replicate.update.-replicate|`mc replicate update --replicate`]] command and omit `replica-metadata-sync` from the replication feature list.

### Replication of Delete Operations

MinIO supports replicating delete operations onto the target bucket.
Specifically, MinIO can replicate both [Delete Markers](https://docs.aws.amazon.com/AmazonS3/latest/userguide/versioning-workflows.html) and the deletion of specific versioned objects:

For delete operations on an object, MinIO replication also creates the delete marker on the target bucket.

For delete operations on versions of an object, MinIO replication also deletes those versions on the target bucket.

MinIO does not replicate objects deleted due to [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|lifecycle management expiration rules]].
MinIO only replicates explicit client-driven delete operations.

MinIO requires explicitly enabling replication of delete operations using the [`mc replicate add --replicate`](#mc.replicate.add.-replicate) flag.
This procedure includes the required flags for enabling replication of delete operations and delete markers.
See [[administration/bucket-replication#minio-replication-behavior-delete|Replication of Delete Operations]] for more complete documentation on this behavior.

### Replication of Encrypted Objects

MinIO supports replicating objects encrypted with automatic Server-Side Encryption (SSE-S3).
Both the source and destination buckets must have automatic SSE-S3 enabled for MinIO to replicate an encrypted object.

As part of the replication process, MinIO decrypts the object on the source bucket and transmits the unencrypted object.
The destination MinIO deployment then re-encrypts the object using the destination bucket SSE-S3 configuration.
MinIO strongly recommends [[operations/network-encryption#minio-tls|enabling TLS]] on both source and destination deployments to ensure the safety of objects during transmission.

MinIO does not support replicating client-side encrypted objects (SSE-C).

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