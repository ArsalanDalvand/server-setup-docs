---
title: "mc replicate update — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-replicate-update.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-replicate-update"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc replicate update — SILO (MinIO Object Storage)"
---
# mc replicate update

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc replicate update` replaces the `mc admin bucket remote update` command.

Changed in version RELEASE.2022-11-07T23-47-39Z: `mc replicate update` replaces the `mc replicate edit` command.

## Syntax

The [`mc replicate update`](#command-mc.replicate.update) command modifies an existing
[[administration/bucket-replication#minio-bucket-replication-serverside|bucket replication rule]].

```
mc [GLOBALFLAGS] replicate update FLAGS [FLAGS] ARGUMENTS [ARGUMENTS]
```

EXAMPLE

The following command modifies an existing replication rule for the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc replicate update --id "c76um9h4b0t1ijr36mug"           \
   --replicate "delete,delete-marker,existing-objects"  \
   myminio/mydata
```

The new replication configuration synchronizes all versioned delete
operations, delete marker creation, and existing objects to the remote
MinIO deployment.

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] replicate update              \
                 --remote-bucket string          \
                 [--bandwidth "string"]            \
                 [--healthcheck-seconds integer]   \
                 [--id "string"]                   \
                 [--limit-upload "string"]         \
                 [--limit-download "string"]       \
                 [--path "string"]                 \
                 [--priority int]                  \
                 [--proxy]
                 [--replicate "string"]            \
                 [--state string]
                 [--storage-class "string"]        \
                 [--sync string]                          \
                 [--tags "string"]                 \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.replicate.update.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment and full path to the bucket or bucket prefix on which to modify the replication rule.
For example:

```
mc replicate update --id "c75nrap4b0talo3ipthg" [FLAGS]
```

--id[](#mc.replicate.update.-id)
Required

Specify the unique ID for a configured replication rule.
Use the [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]] command to list the replication rules for a bucket.

--bandwidth[](#mc.replicate.update.-bandwidth)
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

--healthcheck-seconds[](#mc.replicate.update.-healthcheck-seconds)
Optional

The length of time in seconds between checks on the health of the remote bucket.

If not specified, MinIO uses an interval of 60 seconds.

--limit-download[](#mc.replicate.update.-limit-download)
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

--limit-upload[](#mc.replicate.update.-limit-upload)
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

--path[](#mc.replicate.update.-path)
Optional

Enable path-style lookup support for the remote bucket.

Valid values include:

`on` - use a path lookup to find the remote bucket

`off` - use a resource locator style (such as a domain or IP address) lookup to find the remote bucket

`auto` - ask MinIO to identify the correct type of lookup to use to find the remote bucket

When not defined, MinIO uses the `auto` value.

--priority[](#mc.replicate.update.-priority)
Optional

Specify the integer priority of the replication rule.
The value must be unique among all other rules on the source bucket.
Higher values imply a higher priority than all other rules.

--proxy[](#mc.replicate.update.-proxy)
Optional

When defining active-active replication between buckets, do not proxy.

Valid values include:

`enable` - Enable proxying in active-active replication.

`disable` - Disable proxying in active-active replication.

By default, MinIO defaults to `enable`.

--remote-bucket[](#mc.replicate.update.-remote-bucket)
Optional

Specify the credentials, destination deployment, and bucket of the remote location.
Value may be an [[reference/minio-mc/mc-alias-set#alias|alias]] and bucket, location based (IP or URL), or path based.

For example, a URL based target might look like the following:

```
--remote-bucket https://user:secret@myminio.cloudprovider.tld:9001/bucket
```

An alias based target might look like the following:

```
--remote-bucket minio-target/my-bucket
```

--replicate[](#mc.replicate.update.-replicate)
Optional

Specify a comma-separated list of the following values to enable extended replication features:

`delete` - Directs MinIO to replicate [[administration/object-management/object-delete#minio-object-delete|DELETE operations]] to the destination bucket.

`delete-marker` - Directs MinIO to replicate delete markers to the destination bucket.

`replica-metadata-sync` - Directs MinIO to synchronize metadata-only changes on a replicated object back to the source.
This feature only effects [[administration/bucket-replication/enable-server-side-two-way-bucket-replication#minio-bucket-replication-serverside-twoway|two-way active-active]] replication configurations.

Omitting this value directs MinIO to stop replicating metadata-only changes back to the source.

`existing-objects` - Directs MinIO to replicate objects created prior to configuring or enabling replication.
MinIO by default does not synchronize existing objects to the remote target.

See [[administration/bucket-replication#minio-replication-behavior-existing-objects|Replication of Existing Objects]] for more information.

--state[](#mc.replicate.update.-state)
Optional

Enables or disables the replication rule.
Specify one of the following values:

`"enable"` - Enables the replication rule.

`"disable"` - Disables the replication rule.

Objects created while replication is disabled are not immediately eligible for replication after enabling the rule.
You must explicitly enable replication of existing objects by including `"existing-objects"` to the list of replication features specified to [`mc replicate update --replicate`](#mc.replicate.update.-replicate).

See [[administration/bucket-replication#minio-replication-behavior-existing-objects|Replication of Existing Objects]] for more information.

--storage-class[](#mc.replicate.update.-storage-class)
Optional

Specify the MinIO [[reference/minio-server/settings/storage-class#minio-ec-storage-class|storage class]] to apply to replicated objects.

--sync[](#mc.replicate.update.-sync)
Optional

Enable synchronous replication for this remote target.

By default, MinIO uses asynchronous replication.

--tags[](#mc.replicate.update.-tags)
Optional

Specify one or more ampersand `&` separated key-value pair tags which MinIO uses for filtering objects to replicate. For example:

```
mc replicate update --id "ID" --tags "TAG1=VALUE&TAG2=VALUE&TAG3=VALUE"
```

MinIO applies the replication rule to any object whose tag set
contains the specified replication tags.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Modify an Existing Replication Rule

Use [`mc replicate update`](#command-mc.replicate.update) to modify an existing replication rule.

```
mc replicate update ALIAS/PATH \
   --id ID                     \
   [--FLAGS]
```

Replace [`ALIAS`](#mc.replicate.update.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`PATH`](#mc.replicate.update.ALIAS) with the path to the bucket or bucket prefix on which the rule exists.

Replace [`ID`](#mc.replicate.update.-id) with the unique identifier for the rule to modify.
Use [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]] to retrieve the list of replication rules on the bucket and their corresponding identifiers.

Note

Modifying a replication configuration rule does not affect already replicated objects.
For example, modifying the [`--tags`](#mc.replicate.update.-tags) filter does not result in the removal of replicated objects which do not meet the filter.

### Update the Credentials for an Existing Replication Rule

Use [`mc replicate update`](#command-mc.replicate.update) to modify an existing replication rule.

```
mc replicate update ALIAS/PATH \
   --id ID                     \
   --remote-bucket https://user:[email protected]:9001/mybucket
```

Replace [`ALIAS`](#mc.replicate.update.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`PATH`](#mc.replicate.update.ALIAS) with the path to the bucket or bucket prefix on which the rule exists.

Replace [`ID`](#mc.replicate.update.-remote-bucket) with the updated credentials, path, and bucket.

### Disable or Enable an Existing Replication Rule

Use [`mc replicate update`](#command-mc.replicate.update) with the [`--state`](#mc.replicate.update.-state) flag to disable or enable a replication rule.

```
mc replicate update ALIAS/PATH \
   --id ID \
   --state "disable"|"enable"
```

Replace [`ALIAS`](#mc.replicate.update.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`PATH`](#mc.replicate.update.ALIAS) with the path to the bucket or bucket prefix on which the rule exists.

Replace [`ID`](#mc.replicate.update.-id) with the unique identifier for the rule to modify.
Use [[reference/minio-mc/mc-replicate-ls#command-mc.replicate.ls|`mc replicate ls`]] to retrieve the list of replication rules on the bucket and their corresponding identifiers.

Specify either `"disable"` or `"enable"` to the [`--state`](#mc.replicate.update.-state) flag to disable or enable the replication rule.

Note

MinIO requires enabling [[administration/bucket-replication#minio-replication-behavior-existing-objects|existing object replication]] to synchronize objects written or removed after disabling a replication rule.

For rules without existing object replication, MinIO synchronizes only those write or delete operations issued while the replication rule is enabled.

## Behavior

### Required Permissions

MinIO strongly recommends creating users specifically for supporting bucket replication operations.
See [[reference/minio-mc-admin/mc-admin-user#command-mc.admin.user|`mc admin user`]] and [[reference/minio-mc-admin/mc-admin-policy#command-mc.admin.policy|`mc admin policy`]] for more complete documentation on adding users and policies to a MinIO deployment.

Replication Admin

The following policy provides permissions for configuring and enabling
replication on a deployment.

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

The `"EnableRemoteBucketConfiguration"` statement grants permission
for creating a remote target for supporting replication.

The `"EnableReplicationRuleConfiguration"` statement grants permission
for creating replication rules on a bucket. The `"arn:aws:s3:::*`
resource applies the replication permissions to any bucket on the
source deployment. You can restrict the user policy to specific buckets
as-needed.

Use the [[reference/minio-mc-admin/mc-admin-policy-create#command-mc.admin.policy.create|`mc admin policy create`]] to add this policy to each
deployment acting as a replication source. Use [[reference/minio-mc-admin/mc-admin-user-add#command-mc.admin.user.add|`mc admin user add`]]
to create a user on the deployment and [[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`mc admin policy attach`]]
to associate the policy to that new user.

Replication Remote User

The following policy provides permissions for enabling synchronization of
replicated data into the deployment.

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

The `"EnableReplicationOnBucket"` statement grants permission for
a remote target to retrieve bucket-level configuration for supporting
replication operations on all buckets in the MinIO deployment. To
restrict the policy to specific buckets, specify those buckets as an
element in the `Resource` array similar to
`"arn:aws:s3:::bucketName"`.

The `"EnableReplicatingDataIntoBucket"` statement grants permission
for a remote target to synchronize data into any bucket in the MinIO
deployment. To restrict the policy to specific buckets, specify those
buckets as an element in the `Resource` array similar to
`"arn:aws:s3:::bucketName/*"`.

Use the [[reference/minio-mc-admin/mc-admin-policy-create#command-mc.admin.policy.create|`mc admin policy create`]] to add this policy to each
deployment acting as a replication target. Use [[reference/minio-mc-admin/mc-admin-user-add#command-mc.admin.user.add|`mc admin user add`]]
to create a user on the deployment and [[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`mc admin policy attach`]]
to associate the policy to that new user.

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