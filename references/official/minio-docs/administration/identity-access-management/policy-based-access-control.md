---
title: "Access Management — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/identity-access-management/policy-based-access-control.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/identity-access-management/policy-based-access-control"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Access Management — SILO (MinIO Object Storage)"
---
# Access Management

## Overview

MinIO uses Policy-Based Access Control (PBAC) to define the authorized actions and resources to which an authenticated user has access.
Each policy describes one or more [actions](#minio-policy-actions) and [conditions](#minio-policy-conditions) that outline the permissions of a [[administration/identity-access-management/minio-user-management#minio-users|user]] or [[administration/identity-access-management/minio-group-management#minio-groups|group]] of users.

MinIO PBAC is built for compatibility with AWS IAM policy syntax, structure, and behavior.
The MinIO documentation makes a best-effort to cover IAM-specific behavior and functionality.
Consider deferring to the [IAM documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/) for more complete documentation on AWS IAM-specific topics.

The [[reference/minio-mc-admin/mc-admin-policy#command-mc.admin.policy|`mc admin policy`]] command supports creation and management of policies on the MinIO deployment.
See the command reference for examples of usage.

## Tag-Based Policy Conditions

Changed in version RELEASE.2022-10-02T19-29-29Z: Policies can use conditions to limit a user’s access only to objects with a [[administration/object-management#minio-object-tagging|specific tag]].

MinIO supports [tag-based conditionals](https://docs.aws.amazon.com/AmazonS3/latest/userguide/tagging-and-policies.html) for policies for [selected actions](#minio-selected-conditional-actions).
Use the `s3:ExistingObjectTag/<key>` in the `Condition` statement of the policy.

## Built-In Policies

MinIO provides the following built-in policies for assigning to
[[administration/identity-access-management/minio-user-management#minio-users|users]] or [[administration/identity-access-management/minio-group-management#minio-groups|groups]]:

consoleAdmin[](#userpolicy.consoleAdmin)

Grants complete access to all S3 and administrative API operations against
all resources on the MinIO deployment. Equivalent to the following set of
actions:

[`s3:*`](#policy-action.s3)

[`admin:*`](#policy-action.admin)

readonly[](#userpolicy.readonly)

Grants read-only permissions on any object on the MinIO deployment. The GET
action must apply to a specific object without requiring any listing.
Equivalent to the following set of actions:

[`s3:GetBucketLocation`](#policy-action.s3-GetBucketLocation)

[`s3:GetObject`](#policy-action.s3-GetObject)

For example, this policy specifically supports GET operations on objects at a
specific path (e.g. `GET play/mybucket/object.file`), such as:

[[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]]

[[reference/minio-mc/mc-stat#command-mc.stat|`mc stat`]]

[[reference/minio-mc/mc-head#command-mc.head|`mc head`]]

[[reference/minio-mc/mc-cat#command-mc.cat|`mc cat`]]

The exclusion of listing permissions is intentional, as typical use cases
do not intend for a “read-only” role to have complete discoverability
(listing all buckets and objects) on the object storage resource.

readwrite[](#userpolicy.readwrite)

Grants read and write permissions for all buckets and objects on the
MinIO server. Equivalent to [`s3:*`](#policy-action.s3).

diagnostics[](#userpolicy.diagnostics)

Grants permission to perform diagnostic actions on the MinIO deployment.
Specifically includes the following actions:

[`admin:ServerTrace`](#policy-action.admin-ServerTrace)

[`admin:Profiling`](#policy-action.admin-Profiling)

[`admin:ConsoleLog`](#policy-action.admin-ConsoleLog)

[`admin:ServerInfo`](#policy-action.admin-ServerInfo)

[`admin:TopLocksInfo`](#policy-action.admin-TopLocksInfo)

[`admin:OBDInfo`](#policy-action.admin-OBDInfo)

[`admin:BandwidthMonitor`](#policy-action.admin-BandwidthMonitor)

[`admin:Prometheus`](#policy-action.admin-Prometheus)

writeonly[](#userpolicy.writeonly)

Grants write-only permissions to any namespace (bucket and path to object)
the MinIO deployment. The PUT action must apply to a specific object
location without requiring any listing.
Equivalent to the [`s3:PutObject`](#policy-action.s3-PutObject) action.

Use [[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`mc admin policy attach`]] to associate a policy to a
user or group on a MinIO deployment.

For example, consider the following table of users. Each user is assigned
a [built-in policy](#minio-policy-built-in) or
a supported [action](#minio-policy-actions). The table
describes a subset of operations a client could perform if authenticated
as that user:

User

Policy

Operations

`Operations`

[`readwrite`](#userpolicy.readwrite) on `finance` bucket
[`readonly`](#userpolicy.readonly) on `audit` bucket

`PUT` and `GET` on `finance` bucket.
`GET` on `audit` bucket

`Auditing`

[`readonly`](#userpolicy.readonly) on `audit` bucket

`GET` on `audit` bucket

`Admin`

[`admin:*`](#policy-action.admin)

All [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands.

Each user can access only those resources and operations which are explicitly
granted by the built-in role. MinIO denies access to any other resource or
action by default.

`Deny` overrides `Allow`

MinIO follows the IAM policy evaluation rules where a `Deny` rule overrides
`Allow` rule on the same action/resource. For example, if a user has an
explicitly assigned policy with an `Allow` rule for an action/resource
while one of its groups has an assigned policy with a `Deny` rule for that
action/resource, MinIO would apply only the `Deny` rule.

For more information on IAM policy evaluation logic, see the IAM
documentation on
[Determining Whether a Request is Allowed or Denied Within an Account](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html#policy-eval-denyallow).

## Policy Document Structure

MinIO policy documents use the same schema as
[AWS IAM Policy](https://docs.aws.amazon.com/IAM/latest/UserGuide/access.html) documents.

The following sample document provides a template for creating custom policies for use with a MinIO deployment.
For more complete documentation on IAM policy elements, see the [IAM JSON Policy Elements Reference](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements.html).

The maximum size for any single policy document is 20KiB.
There is no limit to the number of policy documents that can be attached to a user or group.

```
{
   "Version" : "2012-10-17",
   "Statement" : [
      {
         "Effect" : "Allow",
         "Action" : [ "s3:<ActionName>", ... ],
         "Resource" : "arn:aws:s3:::*",
         "Condition" : { ... }
      },
      {
         "Effect" : "Deny",
         "Action" : [ "s3:<ActionName>", ... ],
         "Resource" : "arn:aws:s3:::*",
         "Condition" : { ... }
      }
   ]
}
```

For the `Statement.Action` array, specify one or more [supported S3 API operations](#minio-policy-actions).

For the `Statement.Resource` key, specify the bucket or bucket prefix to which to restrict the policy.
You can use `*` and `?` wildcard characters as per the [S3 Resource Spec](https://docs.aws.amazon.com/AmazonS3/latest/userguide/s3-arn-format.html).

The `*` wildcard may result in unintended application of a policy to multiple buckets or prefixes based on the [[reference/minio-mc#minio-wildcard-matching|pattern match]].
For example, `arn:aws:s3:::data*` would match the buckets `data`, `data_private`, and `data_internal`.
Specifying only `*` as the resource key applies the policy to all buckets and prefixes on the deployment.

For the `Statement.Condition` key, you can specify one or more [supported Conditions](#minio-policy-conditions).

## Supported S3 Policy Actions

MinIO policy documents support a subset of IAM [S3 Action keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/list_amazons3.html#amazons3-actions-as-permissions).
This section also includes any [condition keys](#minio-policy-conditions) supported by a specific action beyond the common set of supported keys.

The following actions control access to common S3 operations.
The remaining subsections document actions for more advanced S3 operations:

s3:*[](#policy-action.s3)

Selector for all MinIO S3 operations.
Applying this action to a given resource allows the user to perform any S3 operation against that resource.

s3:CreateBucket[](#policy-action.s3-CreateBucket)

Controls access to the [CreateBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CreateBucket.html) S3 API
operation.

s3:DeleteBucket[](#policy-action.s3-DeleteBucket)

Controls access to the [DeleteBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucket.html) S3 API
operation.

s3:ForceDeleteBucket[](#policy-action.s3-ForceDeleteBucket)

Controls access to the [DeleteBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucket.html)
S3 API operation for operations with the `x-minio-force-delete` flag.
Required for removing non-empty buckets.

s3:GetBucketLocation[](#policy-action.s3-GetBucketLocation)

Controls access to the [GetBucketLocation](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketLocation.html) S3 API operation.

s3:ListAllMyBuckets[](#policy-action.s3-ListAllMyBuckets)

Controls access to the [ListBuckets](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListBuckets.html) S3 API operation.

s3:DeleteObject[](#policy-action.s3-DeleteObject)

Controls access to the [DeleteObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObject.html) S3 API operation.

s3:GetObject[](#policy-action.s3-GetObject)

Controls access to the [GetObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:x-amz-server-side-encryption
s3:x-amz-server-side-encryption-customer-algorithm
s3:ExistingObjectTag/<key>
s3:versionid
```

s3:GetObjectAttributes[](#policy-action.s3-GetObjectAttributes)

Controls access to the [GetObjectAttributes](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectAttributes.html) S3 API operation.

s3:GetObjectVersionAttributes[](#policy-action.s3-GetObjectVersionAttributes)

Controls access to the [GetObjectAttributes](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectAttributes.html) S3 API operations on versioned objects.

s3:RestoreObject[](#policy-action.s3-RestoreObject)

Controls access to the [RestoreObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_RestoreObject.html) S3 API operation.

s3:ListBucket[](#policy-action.s3-ListBucket)

Controls access to the [ListObjectsV2](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListObjectsV2.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:prefix
s3:delimiter
s3:max-keys
```

s3:PutObject[](#policy-action.s3-PutObject)

Controls access to the [PutObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:x-amz-copy-source
s3:x-amz-server-side-encryption
s3:x-amz-server-side-encryption-customer-algorithm
s3:x-amz-metadata-directive
s3:x-amz-storage-class
s3:versionid
s3:object-lock-retain-until-date
s3:object-lock-mode
s3:object-lock-legal-hold
s3:RequestObjectTagKeys
s3:RequestObjectTag/<key>
```

s3:PutObjectTagging[](#policy-action.s3-PutObjectTagging)

Controls access to the [PutObjectTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectTagging.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
s3:RequestObjectTagKeys
s3:RequestObjectTag/<key>
```

s3:GetObjectTagging[](#policy-action.s3-GetObjectTagging)

Controls access to the [GetObjectTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectTagging.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
```

s3:DeleteObjectTagging[](#policy-action.s3-DeleteObjectTagging)

Controls access to the [DeleteObjectTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObjectTagging.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
```

### Bucket Configuration

s3:GetBucketPolicy[](#policy-action.s3-GetBucketPolicy)

Controls access to the [GetBucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketPolicy.html) S3 API operation.

s3:PutBucketPolicy[](#policy-action.s3-PutBucketPolicy)

Controls access to the [PutBucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketPolicy.html)
S3 API operation.

s3:DeleteBucketPolicy[](#policy-action.s3-DeleteBucketPolicy)

Controls access to the [DeleteBucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucketPolicy.html) S3 API operation.

s3:GetBucketTagging[](#policy-action.s3-GetBucketTagging)

Controls access to the [GetBucketTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketTagging.html) S3 API operation.

s3:PutBucketTagging[](#policy-action.s3-PutBucketTagging)

Controls access to the [PutBucketTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketTagging.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:RequestObjectTagKeys
s3:RequestObjectTag/<key>
```

s3:GetBucketPolicyStatus[](#policy-action.s3-GetBucketPolicyStatus)

Controls access to the [GetBucketPolicyStatus](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketPolicyStatus.html) S3 API operation.

### Multipart Upload

s3:AbortMultipartUpload[](#policy-action.s3-AbortMultipartUpload)

Controls access to the [AbortMultipartUpload](https://docs.aws.amazon.com/AmazonS3/latest/API/API_AbortMultipartUpload.html) S3 API operation.

s3:ListMultipartUploadParts[](#policy-action.s3-ListMultipartUploadParts)

Controls access to the [ListParts](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListParts.html) S3 API
operation.

s3:ListBucketMultipartUploads[](#policy-action.s3-ListBucketMultipartUploads)

Controls access to the [ListMultipartUploads](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListMultipartUploads.html) S3 API operation.

### Versioning and Retention

s3:PutBucketVersioning[](#policy-action.s3-PutBucketVersioning)

Controls access to the [PutBucketVersioning](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketVersioning.html) S3 API operation.

s3:GetBucketVersioning[](#policy-action.s3-GetBucketVersioning)

Controls access to the [GetBucketVersioning](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketVersioning.html) S3 API operation.

s3:DeleteObjectVersion[](#policy-action.s3-DeleteObjectVersion)

Controls access to the [DeleteObjectVersion](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObjectVersion.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
```

s3:ListBucketVersions[](#policy-action.s3-ListBucketVersions)

Controls access to the [ListBucketVersions](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListBucketVersions.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:prefix
s3:delimiter
s3:max-keys
```

s3:PutObjectVersionTagging[](#policy-action.s3-PutObjectVersionTagging)

Controls access to the [PutObjectVersionTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectVersionTagging.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
s3:RequestObjectTagKeys
s3:RequestObjectTag/<key>
```

s3:GetObjectVersionTagging[](#policy-action.s3-GetObjectVersionTagging)

Controls access to the [GetObjectVersionTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectVersionTagging.html) S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
```

s3:DeleteObjectVersionTagging[](#policy-action.s3-DeleteObjectVersionTagging)

Controls access to the [DeleteObjectVersionTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObjectVersionTagging.html)  S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
```

s3:GetObjectVersion[](#policy-action.s3-GetObjectVersion)

Controls access to the [GetObjectVersion](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectVersion.html)  S3 API operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
```

s3:BypassGovernanceRetention[](#policy-action.s3-BypassGovernanceRetention)

Controls access to the following S3 API operations on objects locked under [[reference/minio-mc/mc-retention-set#mc.retention.set.MODE|`GOVERNANCE`]] retention mode:

`s3:PutObjectRetention`

`s3:PutObject`

`s3:DeleteObject`

See the S3 documentation on [s3:BypassGovernanceRetention](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-managing.html#object-lock-managing-bypass) for more information.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:object-lock-remaining-retention-days
s3:object-lock-retain-until-date
s3:object-lock-mode
s3:object-lock-legal-hold
s3:RequestObjectTagKeys
s3:RequestObjectTag/<key>
```

s3:PutObjectRetention[](#policy-action.s3-PutObjectRetention)

Controls access to the [PutObjectRetention](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectRetention.html)  S3 API operation.

Required for any `PutObject` operation that specifies [[administration/object-management/object-retention#minio-object-locking|retention metadata]].

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:x-amz-server-side-encryption
s3:x-amz-server-side-encryption-customer-algorithm
s3:x-amz-object-lock-remaining-retention-days
s3:x-amz-object-lock-retain-until-date
s3:x-amz-object-lock-mode
s3:versionid
```

s3:GetObjectRetention[](#policy-action.s3-GetObjectRetention)

Controls access to the [GetObjectRetention](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectRetention.html) S3 API operation.

Required for including [[administration/object-management/object-retention#minio-object-locking|object locking metadata]] as part of the response to a `GetObject` or `HeadObject` operation.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:x-amz-server-side-encryption
s3:x-amz-server-side-encryption-customer-algorithm
s3:versionid
```

s3:GetObjectLegalHold[](#policy-action.s3-GetObjectLegalHold)

Controls access to the [GetObjectLegalHold](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectLegalHold.html) S3 API operation.

Required for including [[administration/object-management/object-retention#minio-object-locking|object locking metadata]] as part of the response to a `GetObject` or `HeadObject` operation.

s3:PutObjectLegalHold[](#policy-action.s3-PutObjectLegalHold)

Controls access to the [PutObjectLegalHold](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectLegalHold.html) S3 API operation.

Required for any `PutObject` operation that specifies [[administration/object-management/object-retention#minio-object-locking|legal hold metadata]].

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:x-amz-server-side-encryption
s3:x-amz-server-side-encryption-customer-algorithm
s3:object-lock-legal-hold
s3:versionid
```

s3:GetBucketObjectLockConfiguration[](#policy-action.s3-GetBucketObjectLockConfiguration)

Controls access to the [GetObjectLockConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectLockConfiguration.html) S3 API operation.

s3:PutBucketObjectLockConfiguration[](#policy-action.s3-PutBucketObjectLockConfiguration)

Controls access to the [PutObjectLockConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectLockConfiguration.html) S3 API operation.

### Bucket Notifications

s3:GetBucketNotification[](#policy-action.s3-GetBucketNotification)

Controls access to the [GetBucketNotification](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketNotification.html) S3 API operation.

s3:PutBucketNotification[](#policy-action.s3-PutBucketNotification)

Controls access to the [PutBucketNotification](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotification.html) S3 API operation.

s3:ListenNotification[](#policy-action.s3-ListenNotification)

MinIO Extension for controlling API operations related to MinIO Bucket Notifications.

This action is not intended for use with other S3-compatible services.

s3:ListenBucketNotification[](#policy-action.s3-ListenBucketNotification)

MinIO Extension for controlling API operations related to MinIO Bucket Notifications.

This action is not intended for use with other S3-compatible services.

### Object Lifecycle Management

s3:PutLifecycleConfiguration[](#policy-action.s3-PutLifecycleConfiguration)

Controls access to the [PutLifecycleConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketLifecycleConfiguration.html) S3 API operation.

s3:GetLifecycleConfiguration[](#policy-action.s3-GetLifecycleConfiguration)

Controls access to the [GetLifecycleConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketLifecycleConfiguration.html) S3 API operation.

### Object Encryption

s3:PutEncryptionConfiguration[](#policy-action.s3-PutEncryptionConfiguration)

Controls access to the [PutEncryptionConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketEncryption.html) S3 API operation.

s3:GetEncryptionConfiguration[](#policy-action.s3-GetEncryptionConfiguration)

Controls access to the [GetEncryptionConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketEncryption.html) S3 API operation.

### Bucket Replication

s3:GetReplicationConfiguration[](#policy-action.s3-GetReplicationConfiguration)

Controls access to the [GetBucketReplication](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketReplication.html) S3 API operation.

s3:PutReplicationConfiguration[](#policy-action.s3-PutReplicationConfiguration)

Controls access to the [PutBucketReplication](https://docs.aws.amazon.com/AmazonS3/latest/API/PutBucketReplication.html) S3 API operation.

s3:ReplicateObject[](#policy-action.s3-ReplicateObject)

MinIO Extension for controlling API operations related to [[administration/bucket-replication#minio-bucket-replication-serverside|Server-Side Bucket Replication]].

Required for MinIO server-side replication.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
```

s3:ReplicateDelete[](#policy-action.s3-ReplicateDelete)

MinIO Extension for controlling API operations related to [[administration/bucket-replication#minio-bucket-replication-serverside|Server-Side Bucket Replication]].

Required for synchronizing [[administration/object-management/object-delete#minio-object-delete|delete operations]] as part of MinIO server-side replication.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
```

s3:ReplicateTags[](#policy-action.s3-ReplicateTags)

MinIO Extension for controlling API operations related to [[administration/bucket-replication#minio-bucket-replication-serverside|Server-Side Bucket Replication]].

Required for MinIO server-side replication.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
```

s3:GetObjectVersionForReplication[](#policy-action.s3-GetObjectVersionForReplication)

MinIO Extension for controlling API operations related to [[administration/bucket-replication#minio-bucket-replication-serverside|Server-Side Bucket Replication]].

Required for MinIO server-side replication.

Supports the following additional [condition keys](#minio-policy-conditions):

```
s3:versionid
s3:ExistingObjectTag/<key>
```

## Supported S3 Policy Condition Keys

MinIO policy documents support IAM [conditional statements](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html).

Each condition element consists of [operators](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition_operators.html) and condition keys. MinIO supports a subset of IAM condition keys.
For complete information on any listed condition key, see the [IAM Condition Element Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html)

MinIO supports the following condition keys for all supported
[actions](#minio-policy-actions):

`aws:Referer`

`aws:SourceIp`

`aws:UserAgent`

`aws:SecureTransport`

`aws:CurrentTime`

`aws:EpochTime`

`aws:PrincipalType`

`aws:userid`

`aws:username`

`x-amz-content-sha256`

`s3:signatureAge`

Warning

The `aws:Referer`, `aws:SourceIp`, and `aws.UserAgent` keys may be easily spoofed and therefore pose a potential security risk.
MinIO recommends only using these condition keys to deny access as a secondary security measure.

Never use these three keys to grant access by themselves.

For additional keys supported by a specific S3 action, see the reference documentation for that action.

### MinIO Extended Condition Keys

MinIO extends the S3 standard condition keys with the following extended key:

`sts:DurationSeconds`

New in version MinIO: SERVER RELEASE.2024-02-06T21-36-22Z

Specify a time in seconds to limit the duration of all Security Token Service credentials generated by [[developers/security-token-service/AssumeRoleWithWebIdentity#minio-sts-assumerolewithwebidentity|AssumeRoleWithWebIdentity]].

This value overrides the `DurationSeconds` field specified to the client.

For example:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
            "Effect": "Allow",
            "Action": [
               "sts:AssumeRoleWithWebIdentity"
            ],
            "Condition": {
               "NumericLessThanEquals": {
                  "sts:DurationSeconds": "300"
               }
            }
      }
   ]
}
```

## mc admin Policy Action Keys

MinIO supports the following actions for use with defining policies
for [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] operations. These actions are only valid for
MinIO deployments and are not intended for use with other S3-compatible
services:

admin:*[](#policy-action.admin)

Selector for all admin action keys.

admin:Heal[](#policy-action.admin-Heal)

Allows heal command

admin:StorageInfo[](#policy-action.admin-StorageInfo)

Allows listing server info

admin:DataUsageInfo[](#policy-action.admin-DataUsageInfo)

Allows listing data usage info

admin:TopLocksInfo[](#policy-action.admin-TopLocksInfo)

Allows listing top locks

admin:Profiling[](#policy-action.admin-Profiling)

Allows profiling

admin:ServerTrace[](#policy-action.admin-ServerTrace)

Allows listing server trace

admin:ConsoleLog[](#policy-action.admin-ConsoleLog)

Allows listing console logs on terminal

admin:KMSCreateKey[](#policy-action.admin-KMSCreateKey)

Allows creating a new KMS master key

While this option is still supported, [`kms:CreateKey`](#policy-action.kms-CreateKey) is preferred.

admin:KMSKeyStatus[](#policy-action.admin-KMSKeyStatus)

Allows getting KMS key status

While this option is still supported, [`kms:KeyStatus`](#policy-action.kms-KeyStatus) is preferred.

admin:ServerInfo[](#policy-action.admin-ServerInfo)

Allows listing server info

admin:OBDInfo[](#policy-action.admin-OBDInfo)

Allows obtaining cluster on-board diagnostics

admin:ServerUpdate[](#policy-action.admin-ServerUpdate)

Allows MinIO binary update

admin:ServiceRestart[](#policy-action.admin-ServiceRestart)

Allows restart of MinIO service.

admin:ServiceStop[](#policy-action.admin-ServiceStop)

Allows stopping MinIO service.

admin:ConfigUpdate[](#policy-action.admin-ConfigUpdate)

Allows MinIO config management

admin:CreateUser[](#policy-action.admin-CreateUser)

Allows creating MinIO user

admin:DeleteUser[](#policy-action.admin-DeleteUser)

Allows deleting MinIO user

admin:ListUsers[](#policy-action.admin-ListUsers)

Allows list users permission

admin:EnableUser[](#policy-action.admin-EnableUser)

Allows enable user permission

admin:DisableUser[](#policy-action.admin-DisableUser)

Allows disable user permission

admin:GetUser[](#policy-action.admin-GetUser)

Allows GET permission on user info

admin:AddUserToGroup[](#policy-action.admin-AddUserToGroup)

Allows adding user to group permission

admin:RemoveUserFromGroup[](#policy-action.admin-RemoveUserFromGroup)

Allows removing user to group permission

admin:GetGroup[](#policy-action.admin-GetGroup)

Allows getting group info

admin:ListGroups[](#policy-action.admin-ListGroups)

Allows list groups permission

admin:EnableGroup[](#policy-action.admin-EnableGroup)

Allows enable group permission

admin:DisableGroup[](#policy-action.admin-DisableGroup)

Allows disable group permission

admin:CreatePolicy[](#policy-action.admin-CreatePolicy)

Allows create policy permission

admin:DeletePolicy[](#policy-action.admin-DeletePolicy)

Allows delete policy permission

admin:GetPolicy[](#policy-action.admin-GetPolicy)

Allows get policy permission

admin:AttachUserOrGroupPolicy[](#policy-action.admin-AttachUserOrGroupPolicy)

Allows attaching a policy to a user/group

admin:ListUserPolicies[](#policy-action.admin-ListUserPolicies)

Allows listing user policies

admin:CreateServiceAccount[](#policy-action.admin-CreateServiceAccount)

Allows creating MinIO Access Key

admin:UpdateServiceAccount[](#policy-action.admin-UpdateServiceAccount)

Allows updating MinIO Access Key

admin:RemoveServiceAccount[](#policy-action.admin-RemoveServiceAccount)

Allows deleting MinIO Access Key

admin:ListServiceAccounts[](#policy-action.admin-ListServiceAccounts)

Allows listing MinIO Access Key

admin:SetBucketQuota[](#policy-action.admin-SetBucketQuota)

Allows setting bucket quota

admin:GetBucketQuota[](#policy-action.admin-GetBucketQuota)

Allows getting bucket quota

admin:SetBucketTarget[](#policy-action.admin-SetBucketTarget)

Allows setting bucket target

admin:GetBucketTarget[](#policy-action.admin-GetBucketTarget)

Allows getting bucket targets

admin:SetTier[](#policy-action.admin-SetTier)

Allows creating and modifying remote storage tiers using the
[[reference/minio-mc/mc-ilm-tier#command-mc.ilm.tier|`mc ilm tier`]] commands.

admin:ListTier[](#policy-action.admin-ListTier)

Allows listing configured remote storage tiers using the
[[reference/minio-mc/mc-ilm-tier#command-mc.ilm.tier|`mc ilm tier`]] commands.

admin:BandwidthMonitor[](#policy-action.admin-BandwidthMonitor)

Allows retrieving metrics related to current bandwidth consumption.

admin:Prometheus[](#policy-action.admin-Prometheus)

Allows access to MinIO [[operations/monitoring/metrics-and-alerts#minio-metrics-and-alerts|metrics]].
Only required if MinIO requires authentication for scraping metrics.

admin:ListBatchJobs[](#policy-action.admin-ListBatchJobs)

Allows access to list the active batch jobs.

admin:DescribeBatchJob[](#policy-action.admin-DescribeBatchJob)

Allows access to the see the definition details of a running batch job.

admin:StartBatchJob[](#policy-action.admin-StartBatchJob)

Allows user to begin a batch job run.

admin:CancelBatchJob[](#policy-action.admin-CancelBatchJob)

Allows user to stop a batch job currently in process.

admin:Rebalance[](#policy-action.admin-Rebalance)

Allows access to start, query, or stop a rebalancing of objects across pools with varying free storage space.

## KMS policy action keys

MinIO supports restricting key management service (KMS) actions by policy.

You can restrict KMS activities in a policy with any of the following KMS actions:

kms:Status[](#policy-action.kms-Status)

Check the status of KMS.

kms:Metrics[](#policy-action.kms-Metrics)

Obtain Prometheus-formatted metrics.

kms:API[](#policy-action.kms-API)

List supported API endpoints.

kms:Version[](#policy-action.kms-Version)

Retrieve the KMS version.

kms:CreateKey[](#policy-action.kms-CreateKey)

Create a new KMS key.

kms:ListKeys[](#policy-action.kms-ListKeys)

Retrieve a list of existing KMS keys.

kms:KeyStatus[](#policy-action.kms-KeyStatus)

Retrieve the status of a specified KMS key.

To select all of the available kms policy actions, use `kms:*`.

Changed in version RELEASE.2024-07-16T23-46-41Z: KMS actions can be restricted by resource or a resource prefix.
The wildcard character `*` can be used to apply the KMS action policy to all resources that match the prefix.

For example, the following policy document allows a user to list keys, create new keys, and check the status of keys for any resource that begins with `keys-abc-` or `myuser-`.

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "kms:CreateKey",
                "kms:KeyStatus",
                "kms:ListKeys"
            ],
            "Resource": [
                "arn:minio:kms:::keys-abc-*",
                "arn:minio:kms:::myuser-*"
            ]
        }
    ]
}
```

## mc admin Policy Condition Keys

MinIO supports the following conditions for use with defining policies for
[[reference/minio-mc-admin#command-mc.admin|`mc admin`]] [actions](#minio-policy-mc-admin-actions).

`aws:Referer`

`aws:SourceIp`

`aws:UserAgent`

`aws:SecureTransport`

`aws:CurrentTime`

`aws:EpochTime`

For complete information on any listed condition key, see the [IAM Condition Element Documentation](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_condition.html).

## Policy Variables

MinIO supports using policy variables for automatically substituting context from the authenticated user and/or the operation into the user’s assigned policy or policies.
Use the `${POLICYVARIABLE}` format to specify the variable to the policy as part of the `Condition` or `Resource` definition.
MinIO policy variables function similarly to [AWS IAM policy elements: Variables and tags](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_variables.html).

Each MinIO [[administration/identity-access-management#minio-authentication-and-identity-management|identity provider]] supports its own set of policy variables:

[MinIO Policy Variables](#minio-policy-variables-internal)

[OpenID Policy Variables](#minio-policy-variables-oidc)

[Active Directory / LDAP Policy Variables](#minio-policy-variables-ad-ldap)

### MinIO Policy Variables

The following table contains a list of recommended policy variables for use in authorizing [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO-managed users]]:

Variable

Description

[aws:referrer](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-referer)

The referrer in the HTTP header for the authenticated API call.

[aws:SourceIp](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-sourceip)

The source IP in the HTTP header for the authenticated API call.

[aws:username](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-username)

The name of the user associated with the authenticated API call.

For example, the following policy uses variables to substitute the authenticated user’s username as part of the `Resource` field such that the user can only access those prefixes which match their username:

```
{
"Version": "2012-10-17",
"Statement": [
      {
         "Action": ["s3:ListBucket"],
         "Effect": "Allow",
         "Resource": ["arn:aws:s3:::mybucket"],
         "Condition": {"StringLike": {"s3:prefix": ["${aws:username}/*"]}}
      },
      {
         "Action": [
         "s3:GetObject",
         "s3:PutObject"
         ],
         "Effect": "Allow",
         "Resource": ["arn:aws:s3:::mybucket/${aws:username}/*"]
      }
   ]
}
```

MinIO replaces the `${aws:username}` variable in the `Resource` field with the username.
MinIO then evaluates the policy and grants or revokes access to the requested API and resource.

### OpenID Policy Variables

The following table contains a list of supported policy variables for use in authorizing [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OIDC-managed users]].

Each variable corresponds to a claim returned as part of the authenticated user’s JWT token:

Variable

Description

`jwt:sub`

Returns the `sub` claim for the user.

`jwt:iss`

Returns the Issuer Identifier claim from the ID token.

`jwt:aud`

Returns the Audience claim from the ID token.

`jwt:jti`

Returns the JWT ID claim from the client authentication information.

`jwt:upn`

Returns the User Principal Name claim from the client authentication information.

`jwt:name`

Returns the `name` claim for the user.

`jwt:groups`

Returns the `groups` claim for the user.

`jwt:given_name`

Returns the `given_name` claim for the user.

`jwt:family_name`

Returns the `family_name` claim for the user.

`jwt:middle_name`

Returns the `middle_name` claim for the user.

`jwt:nickname`

Returns the `nickname` claim for the user.

`jwt:preferred_username`

Returns the `preferred_username` claim for the user.

`jwt:profile`

Returns the `profile` claim for the user.

`jwt:picture`

Returns the `picture` claim for the user.

`jwt:website`

Returns the `website` claim for the user.

`jwt:email`

Returns the `email` claim for the user.

`jwt:gender`

Returns the `gender` claim for the user.

`jwt:birthdate`

Returns the `birthdate` claim for the user.

`jwt:phone_number`

Returns the `phone_number` claim for the user.

`jwt:address`

Returns the `address` claim for the user.

`jwt:scope`

Returns the `scope` claim for the user.

`jwt:client_id`

Returns the `client_id` claim for the user.

See the [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html) document for more information on these scopes.
Your OIDC provider of choice may have more specific documentation.

For example, the following policy uses variables to substitute the authenticated user’s `preferred_username` as part of the `Resource` field such that the user can only access those prefixes which match their username:

```
{
"Version": "2012-10-17",
"Statement": [
      {
         "Action": ["s3:ListBucket"],
         "Effect": "Allow",
         "Resource": ["arn:aws:s3:::mybucket"],
         "Condition": {"StringLike": {"s3:prefix": ["${jwt:preferred_username}/*"]}}
      },
      {
         "Action": [
         "s3:GetObject",
         "s3:PutObject"
         ],
         "Effect": "Allow",
         "Resource": ["arn:aws:s3:::mybucket/${jwt:preferred_username}/*"]
      }
   ]
}
```

MinIO replaces the `${jwt:preferred_username}` variable in the `Resource` field with the value of the `preferred_username` in the JWT token.
MinIO then evaluates the policy and grants or revokes access to the requested API and resource.

### Active Directory / LDAP Policy Variables

The following table contains a list of supported policy variables for use in authorizing [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|AD/LDAP users]]:

Variable

Description

`ldap:username`

The simple username (`name`) for the authenticated user.

This is distinct from the user’s DistinguishedName or CommonName.

`ldap:user`

The Distinguished Name used by the authenticated user.

`ldap:groups`

The Group Distinguished Name for the authenticated user.

For example, the following policy uses variables to substitute the authenticated user’s `name` as part of the `Resource` field such that the user can only access those prefixes which match their name:

```
{
"Version": "2012-10-17",
"Statement": [
      {
         "Action": ["s3:ListBucket"],
         "Effect": "Allow",
         "Resource": ["arn:aws:s3:::mybucket"],
         "Condition": {"StringLike": {"s3:prefix": ["${ldap:username}/*"]}}
      },
      {
         "Action": [
         "s3:GetObject",
         "s3:PutObject"
         ],
         "Effect": "Allow",
         "Resource": ["arn:aws:s3:::mybucket/${ldap:username}/*"]
      }
   ]
}
```

MinIO replaces the `${ldap:username}` variable in the `Resource` field with the value of the authenticated user’s `name`.
MinIO then evaluates the policy and grants or revokes access to the requested API and resource.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).