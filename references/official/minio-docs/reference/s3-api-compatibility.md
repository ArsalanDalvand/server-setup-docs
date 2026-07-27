---
title: "S3 API Compatibility — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/s3-api-compatibility.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/s3-api-compatibility"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "S3 API Compatibility — SILO (MinIO Object Storage)"
---
# S3 API Compatibility

This page documents S3 APIs supported by MinIO Object Storage.
For reference documentation on any given API, see the corresponding documentation for Amazon S3.

Important

MinIO strongly recommends using an [[developers/minio-drivers#minio-drivers|S3-Compatible SDK]] for performing object storage operations.

## Object APIs

[CopyObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CopyObject.html)

[DeleteObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObject.html)

[DeleteObjects](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObjects.html)

[DeleteObjectTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteObjectTagging.html)

[GetObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObject.html)

[GetObjectAttributes](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectAttributes.html)

[GetObjectTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectTagging.html)

[HeadObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_HeadObject.html)

[ListObjects](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListObjects.html)

[ListObjectsV2](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListObjectsV2.html)

[ListObjectVersions](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListObjectVersions.html)

[PutObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObject.html)

[PutObjectTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectTagging.html)

[RestoreObject](https://docs.aws.amazon.com/AmazonS3/latest/API/API_RestoreObject.html)

[SelectObjectContent](https://docs.aws.amazon.com/AmazonS3/latest/API/API_SelectObjectContent.html)

### Object Locking

[GetObjectRetention](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectRetention.html)

[PutObjectRetention](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectRetention.html)

[GetObjectLegalHold](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectLegalHold.html)

[PutObjectLegalHold](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectLegalHold.html)

[GetObjectLockConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetObjectLockConfiguration.html)

[PutObjectLockConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutObjectLockConfiguration.html)

### Unsupported API Object Endpoints

```
GetObjectAcl
PutObjectAcl
```

### Multipart Uploads

[AbortMultipartUpload](https://docs.aws.amazon.com/AmazonS3/latest/API/API_AbortMultipartUpload.html)

[CompleteMultipartUpload](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CompleteMultipartUpload.html)

[CreateMultipartUpload](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CreateMultipartUpload.html)

[ListMultipartUploads](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListMultipartUploads.html)

[ListParts](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListParts.html)

[UploadPart](https://docs.aws.amazon.com/AmazonS3/latest/API/API_UploadPart.html)

[UploadPartCopy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_UploadPartCopy.html)

#### Differences from S3 APIs for Multipart Uploads

`ListMultipartUploads` requires the exact object name as a prefix.

The `AbortIncompleteMultipartUpload` lifecycle action is not supported with `PutBucketLifecycle`.

## Bucket APIs

[CreateBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_CreateBucket.html)

[DeleteBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucket.html)

[DeleteBucketEncryption](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucketEncryption.html)

[DeleteBucketTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucketTagging.html)

[GetBucketEncryption](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketEncryption.html)

[GetBucketLocation](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketLocation.html)

[GetBucketTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketTagging.html)

[GetBucketVersioning](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketVersioning.html)

[HeadBucket](https://docs.aws.amazon.com/AmazonS3/latest/API/API_HeadBucket.html)

[ListBuckets](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListBuckets.html)

[ListDirectoryBuckets](https://docs.aws.amazon.com/AmazonS3/latest/API/API_ListDirectoryBuckets.html)

[PutBucketEncryption](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketEncryption.html)

[PutBucketTagging](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketTagging.html)

[PutBucketVersioning](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketVersioning.html)

### Bucket Replication

[GetBucketReplication](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketReplication.html)

[PutBucketReplication](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketReplication.html)

[DeleteBucketReplication](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucketReplication.html)

### Bucket Lifecycle

[GetBucketLifecycle](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketLifecycle.html)

[GetBucketLifecycleConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketLifecycleConfiguration.html)

[PutBucketLifecycle](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketLifecycle.html)

[PutBucketLifecycleConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketLifecycleConfiguration.html)

[DeleteBucketLifecycle](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucketLifecycle.html)

### Bucket Notifications

[GetBucketNotification](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketNotification.html)

[GetBucketNotificationConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketNotificationConfiguration.html)

[PutBucketNotification](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotification.html)

[PutBucketNotificationConfiguration](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketNotificationConfiguration.html)

### Bucket Policies

[GetBucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketPolicy.html)

[GetBucketPolicyStatus](https://docs.aws.amazon.com/AmazonS3/latest/API/API_GetBucketPolicyStatus.html)

[PutBucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_PutBucketPolicy.html)

[DeleteBucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/API/API_DeleteBucketPolicy.html)

### Unsupported API Bucket Operations

```
GetBucketInventoryConfiguration
PutBucketInventoryConfiguration
DeleteBucketInventoryConfiguration
PutBucketCors
DeleteBucketCors
GetBucketMetricsConfiguration
PutBucketMetricsConfiguration
DeleteBucketMetricsConfiguration
PutBucketWebsite
GetBucketLogging
PutBucketLogging
PutBucketAccelerateConfiguration
DeleteBucketAccelerateConfiguration
PutBucketRequestPayment
DeleteBucketRequestPayment
PutBucketAcl
HeadBucketAcl
GetPublicAccessBlock
PutPublicAccessBlock
DeletePublicAccessBlock
GetBucketOwnershipControls
PutBucketOwnershipControls
DeleteBucketOwnershipControls
GetBucketIntelligentTieringConfiguration
PutBucketIntelligentTieringConfiguration
ListBucketIntelligentTieringConfigurations
DeleteBucketIntelligentTieringConfiguration
GetBucketAnalyticsConfiguration
PutBucketAnalyticsConfiguration
ListBucketAnalyticsConfigurations
DeleteBucketAnalyticsConfiguration
CreateSession
```

### MinIO alternatives for unsupported Bucket resources

For calls to `BucketACL` or `ObjectACL` operations, use [[administration/identity-access-management/policy-based-access-control#minio-policy|Policies]].

Calls to `BucketCORS` operations are not needed because CORS is enabled by default on all buckets for all HTTP verbs.

For calls to `BucketWebsite` operations, use `caddy` or `nginx`.

For calls to `BucketAnalytics`, `BucketMetrics`, or `BucketLogging` operations, use [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).