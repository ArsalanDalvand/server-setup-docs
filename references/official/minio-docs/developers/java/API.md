---
title: "Java Client API Reference — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/developers/java/API.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "developers/java/API"
category: "developers"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Java Client API Reference — SILO (MinIO Object Storage)"
---
# Java Client API Reference

## Create MinIO Client.

## MinIO

```
MinioClient minioClient =
    MinioClient.builder()
        .endpoint("https://play.min.io")
        .credentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
        .build();
```

## AWS S3

```
MinioClient minioClient =
    MinioClient.builder()
        .endpoint("https://s3.amazonaws.com")
        .credentials("YOUR-ACCESSKEYID", "YOUR-SECRETACCESSKEY")
        .build();
```

Bucket operations

Object operations

[`bucketExists`](#bucketExists)

[`composeObject`](#composeObject)

[`deleteBucketCors`](#deleteBucketCors)

[`copyObject`](#copyObject)

[`deleteBucketEncryption`](#deleteBucketEncryption)

[`deleteObjectTags`](#deleteObjectTags)

[`deleteBucketLifecycle`](#deleteBucketLifecycle)

[`disableObjectLegalHold`](#disableObjectLegalHold)

[`deleteBucketNotification`](#deleteBucketNotification)

[`downloadObject`](#downloadObject)

[`deleteBucketPolicy`](#deleteBucketPolicy)

[`enableObjectLegalHold`](#enableObjectLegalHold)

[`deleteBucketReplication`](#deleteBucketReplication)

[`getObject`](#getObject)

[`deleteBucketTags`](#deleteBucketTags)

[`getObjectAcl`](#getObjectAcl)

[`deleteObjectLockConfiguration`](#deleteObjectLockConfiguration)

[`getObjectAttributes`](#getObjectAttributes)

[`getBucketCors`](#getBucketCors)

[`getObjectRetention`](#getObjectRetention)

[`getBucketEncryption`](#getBucketEncryption)

[`getObjectTags`](#getObjectTags)

[`getBucketLifecycle`](#getBucketLifecycle)

[`getPresignedObjectUrl`](#getPresignedObjectUrl)

[`getBucketNotification`](#getBucketNotification)

[`getPresignedPostFormData`](#getPresignedPostFormData)

[`getBucketPolicy`](#getBucketPolicy)

[`isObjectLegalHoldEnabled`](#isObjectLegalHoldEnabled)

[`getBucketReplication`](#getBucketReplication)

[`listObjects`](#listObjects)

[`getBucketTags`](#getBucketTags)

[`promptObject`](#promptObject)

[`getBucketVersioning`](#getBucketVersioning)

[`putObject`](#putObject)

[`getObjectLockConfiguration`](#getObjectLockConfiguration)

[`putObjectFanOut`](#putObjectFanOut)

[`listBuckets`](#listBuckets)

[`removeObject`](#removeObject)

[`listenBucketNotification`](#listenBucketNotification)

[`removeObjects`](#removeObjects)

[`makeBucket`](#makeBucket)

[`restoreObject`](#restoreObject)

[`removeBucket`](#removeBucket)

[`selectObjectContent`](#selectObjectContent)

[`setBucketCors`](#setBucketCors)

[`setObjectRetention`](#setObjectRetention)

[`setBucketEncryption`](#setBucketEncryption)

[`setObjectTags`](#setObjectTags)

[`setBucketLifecycle`](#setBucketLifecycle)

[`statObject`](#statObject)

[`setBucketNotification`](#setBucketNotification)

[`uploadObject`](#uploadObject)

[`setBucketPolicy`](#setBucketPolicy)

[`uploadSnowballObjects`](#uploadSnowballObjects)

[`setBucketReplication`](#setBucketReplication)

[`setBucketTags`](#setBucketTags)

[`setBucketVersioning`](#setBucketVersioning)

[`setObjectLockConfiguration`](#setObjectLockConfiguration)

## 1. MinIO Client Builder

MinIO Client Builder is used to create MinIO client. Builder has below methods to accept arguments.

Method

Description

`endpoint()`

Accepts endpoint as a String, URL or okhttp3.HttpUrl object and optionally accepts port number and flag to enable secure (TLS) connection.

Endpoint as a string can be formatted like below:

`https://s3.amazonaws.com`

`https://play.min.io`

`https://play.min.io:9000`

`localhost`

`play.min.io`

`credentials()`

Accepts access key (aka user ID) and secret key (aka password) of an account in S3 service.

`region()`

Accepts region name of S3 service. If specified, all operations use this region otherwise region is probed per bucket.

`httpClient()`

Custom HTTP client to override default.

Examples

### MinIO

```
// 1. Create client to S3 service 'play.min.io' at port 443 with TLS security
// for anonymous access.
MinioClient minioClient = MinioClient.builder().endpoint("https://play.min.io").build();

// 2. Create client to S3 service 'play.min.io' at port 443 with TLS security
// using URL object for anonymous access.
MinioClient minioClient = MinioClient.builder().endpoint(new URL("https://play.min.io")).build();

// 3. Create client to S3 service 'play.min.io' at port 9000 with TLS security
// using okhttp3.HttpUrl object for anonymous access.
MinioClient minioClient =
    MinioClient.builder().endpoint(HttpUrl.parse("https://play.min.io:9000")).build();

// 4. Create client to S3 service 'play.min.io' at port 443 with TLS security
// for authenticated access.
MinioClient minioClient =
    MinioClient.builder()
	    .endpoint("https://play.min.io")
		.credentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
		.build();

// 5. Create client to S3 service 'play.min.io' at port 9000 with non-TLS security
// for authenticated access.
MinioClient minioClient =
    MinioClient.builder()
	    .endpoint("play.min.io", 9000, false)
	    .credentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
		.build();

// 6. Create client to S3 service 'play.min.io' at port 9000 with TLS security
// for authenticated access.
MinioClient minioClient =
    MinioClient.builder()
	    .endpoint("play.min.io", 9000, true)
		.credentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
		.build();

// 7. Create client to S3 service 'play.min.io' at port 443 with TLS security
// and region 'us-west-1' for authenticated access.
MinioClient minioClient =
    MinioClient.builder()
	    .endpoint(new URL("https://play.min.io"))
		.credentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
		.region("us-west-1")
		.build();

// 8. Create client to S3 service 'play.min.io' at port 9000 with TLS security,
// region 'eu-east-1' and custom HTTP client for authenticated access.
MinioClient minioClient =
    MinioClient.builder()
	    .endpoint("https://play.min.io:9000")
		.credentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
		.region("eu-east-1")
		.httpClient(customHttpClient)
		.build();
```

### AWS S3

```
// 1. Create client to S3 service 's3.amazonaws.com' at port 443 with TLS security
// for anonymous access.
MinioClient s3Client = MinioClient.builder().endpoint("https://s3.amazonaws.com").build();

// 2. Create client to S3 service 's3.amazonaws.com' at port 443 with TLS security
// using URL object for anonymous access.
MinioClient s3Client = MinioClient.builder().endpoint(new URL("https://s3.amazonaws.com")).build();

// 3. Create client to S3 service 's3.amazonaws.com' at port 9000 with TLS security
// using okhttp3.HttpUrl object for anonymous access.
MinioClient s3Client =
    MinioClient.builder().endpoint(HttpUrl.parse("https://s3.amazonaws.com")).build();

// 4. Create client to S3 service 's3.amazonaws.com' at port 80 with TLS security
// for authenticated access.
MinioClient s3Client =
    MinioClient.builder()
	    .endpoint("s3.amazonaws.com")
		.credentials("YOUR-ACCESSKEYID", "YOUR-SECRETACCESSKEY")
		.build();

// 5. Create client to S3 service 's3.amazonaws.com' at port 443 with non-TLS security
// for authenticated access.
MinioClient s3Client =
    MinioClient.builder()
        .endpoint("s3.amazonaws.com", 433, false)
		.credentials("YOUR-ACCESSKEYID", "YOUR-SECRETACCESSKEY")
		.build();

// 6. Create client to S3 service 's3.amazonaws.com' at port 80 with non-TLS security
// for authenticated access.
MinioClient s3Client =
    MinioClient.builder()
	    .endpoint("s3.amazonaws.com", 80, false)
        .credentials("YOUR-ACCESSKEYID", "YOUR-SECRETACCESSKEY")
		.build();

// 7. Create client to S3 service 's3.amazonaws.com' at port 80 with TLS security
// for authenticated access.
MinioClient s3Client =
    MinioClient.builder()
	    .endpoint("s3.amazonaws.com", 80, true)
        .credentials("YOUR-ACCESSKEYID", "YOUR-SECRETACCESSKEY")
		.build();

// 8. Create client to S3 service 's3.amazonaws.com' at port 80 with non-TLS security
// and region 'us-west-1' for authenticated access.
MinioClient s3Client =
    MinioClient.builder()
	    .endpoint("s3.amazonaws.com", 80, false)
        .credentials("YOUR-ACCESSKEYID", "YOUR-SECRETACCESSKEY")
		.region("us-west-1")
		.build();

// 9. Create client to S3 service 's3.amazonaws.com' at port 443 with TLS security
// and region 'eu-west-2' for authenticated access.
MinioClient s3Client =
    MinioClient.builder()
	    .endpoint("s3.amazonaws.com", 443, true)
		.credentials("YOUR-ACCESSKEYID", "YOUR-SECRETACCESSKEY")
		.region("eu-west-2")
		.build();

// 10. Create client to S3 service 's3.amazonaws.com' at port 443 with TLS security,
// region 'eu-central-1' and custom HTTP client for authenticated access.
MinioClient s3Client =
    MinioClient.builder()
	    .endpoint("s3.amazonaws.com", 443, true)
        .credentials("YOUR-ACCESSKEYID", "YOUR-SECRETACCESSKEY")
		.region("eu-central-1")
		.httpClient(customHttpClient)
		.build();
```

## Common Exceptions

All APIs throw below exceptions in addition to specific to API.

Exception

Cause

ErrorResponseException

Thrown to indicate S3 service returned an error response.

IllegalArgumentException

Throws to indicate invalid argument passed.

InsufficientDataException

Thrown to indicate not enough data available in InputStream.

InternalException

Thrown to indicate internal library error.

InvalidKeyException

Thrown to indicate missing of HMAC SHA-256 library.

InvalidResponseException

Thrown to indicate S3 service returned invalid or no error response.

IOException

Thrown to indicate I/O error on S3 operation.

NoSuchAlgorithmException

Thrown to indicate missing of MD5 or SHA-256 digest library.

ServerException

Thrown to indicate HTTP server error.

XmlParserException

Thrown to indicate XML parsing error.

## 2. Bucket operations

### bucketExists(BucketExistsArgs args)

`public boolean bucketExists(BucketExistsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#bucketExists-io.minio.BucketExistsArgs-)

Checks if a bucket exists.

Parameters

Parameter

Type

Description

`bucketName`

[BucketExistsArgs](http://minio.github.io/minio-java/io/minio/BucketExistsArgs.html)

Arguments.

Returns

boolean - True if the bucket exists.

Example

```
// Check whether 'my-bucketname' exists or not.
boolean found =
  minioClient.bucketExists(BucketExistsArgs.builder().bucket("my-bucketname").build());
if (found) {
  System.out.println("my-bucketname exists");
} else {
  System.out.println("my-bucketname does not exist");
}
```

### deleteBucketCors(DeleteBucketCorsArgs args)

`private void deleteBucketCors(DeleteBucketCorsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketCors-io.minio.DeleteBucketCorsArgs-)

Deletes CORS configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[DeleteBucketCorsArgs](http://minio.github.io/minio-java/io/minio/DeleteBucketCorsArgs.html)

Arguments.

Example

```
minioClient.deleteBucketCors(DeleteBucketCorsArgs.builder().bucket("my-bucketname").build());
```

### deleteBucketEncryption(DeleteBucketEncryptionArgs args)

`private void deleteBucketEncryption(DeleteBucketEncryptionArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketEncryption-io.minio.DeleteBucketEncryptionArgs-)

Deletes encryption configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[DeleteBucketEncryptionArgs](http://minio.github.io/minio-java/io/minio/DeleteBucketEncryptionArgs.html)

Arguments.

Example

```
minioClient.deleteBucketEncryption(
    DeleteBucketEncryptionArgs.builder().bucket("my-bucketname").build());
```

### deleteBucketLifecycle(DeleteBucketLifecycleArgs args)

`private void deleteBucketLifecycle(DeleteBucketLifecycleArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketLifecycle-io.minio.DeleteBucketLifecycleArgs-)

Deletes lifecycle configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[DeleteBucketLifecycleArgs](http://minio.github.io/minio-java/io/minio/DeleteBucketLifecycleArgs.html)

Arguments.

Example

```
minioClient.deleteBucketLifecycle(
    DeleteBucketLifecycleArgs.builder().bucket("my-bucketname").build());
```

### deleteBucketTags(DeleteBucketTagsArgs args)

`private void deleteBucketTags(DeleteBucketTagsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketTags-io.minio.DeleteBucketTagsArgs-)

Deletes tags of a bucket.

Parameters

Parameter

Type

Description

`args`

[DeleteBucketTagsArgs](http://minio.github.io/minio-java/io/minio/DeleteBucketTagsArgs.html)

Arguments.

Example

```
minioClient.deleteBucketTags(DeleteBucketTagsArgs.builder().bucket("my-bucketname").build());
```

### deleteBucketPolicy(DeleteBucketPolicyArgs args)

`private void deleteBucketPolicy(DeleteBucketPolicyArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketPolicy-io.minio.DeleteBucketPolicyArgs-)

Deletes bucket policy configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[DeleteBucketPolicyArgs](http://minio.github.io/minio-java/io/minio/DeleteBucketPolicyArgs.html)

Arguments.

Example

```
minioClient.deleteBucketPolicy(DeleteBucketPolicyArgs.builder().bucket("my-bucketname").build());
```

### deleteBucketReplication(DeleteBucketReplicationArgs args)

`private void deleteBucketReplication(DeleteBucketReplicationArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketReplication-io.minio.DeleteBucketReplicationArgs-)

Deletes bucket replication configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[DeleteBucketReplicationArgs](http://minio.github.io/minio-java/io/minio/DeleteBucketReplicationArgs.html)

Arguments.

Example

```
minioClient.deleteBucketReplication(
    DeleteBucketReplicationArgs.builder().bucket("my-bucketname").build());
```

### deleteBucketNotification(DeleteBucketNotificationArgs args)

`public void deleteBucketNotification(DeleteBucketNotificationArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteBucketNotification-io.minio.DeleteBucketNotificationArgs-)

Deletes notification configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[DeleteBucketNotificationArgs](http://minio.github.io/minio-java/io/minio/DeleteBucketNotificationArgs.html)

Arguments.

Example

```
minioClient.deleteBucketNotification(
    DeleteBucketNotificationArgs.builder().bucket("my-bucketname").build());
```

### deleteObjectLockConfiguration(DeleteObjectLockConfigurationArgs args)

`public void deleteObjectLockConfiguration(DeleteObjectLockConfigurationArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteObjectLockConfiguration-io.minio.DeleteObjectLockConfigurationArgs-)

Deletes object-lock configuration in a bucket.

Parameters

Parameter

Type

Description

`args`

[DeleteObjectLockConfigurationArgs](http://minio.github.io/minio-java/io/minio/DeleteObjectLockConfigurationArgs.html)

Arguments.

Example

```
minioClient.deleteObjectLockConfiguration(
    DeleteObjectLockConfigurationArgs.builder().bucket("my-bucketname").build());
```

### getBucketCors(GetBucketCorsArgs args)

`public Tags getBucketCors(GetBucketCorsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.htmlgetBucketCors-io.minio.GetBucketCorsArgs-)

Gets CORS configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[GetBucketCorsArgs](http://minio.github.io/minio-java/io/minio/GetBucketCorsArgs.html)

Arguments.

Returns

[CORSConfiguration] - CORS configuration.

Example

```
CORSConfiguration config = minioClient.getBucketCors(GetBucketCorsArgs.builder().bucket("my-bucketname").build());
```

### getBucketEncryption(GetBucketEncryptionArgs args)

`public SseConfiguration getBucketEncryption(GetBucketEncryptionArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getBucketEncryption-io.minio.GetBucketEncryptionArgs-)

Gets encryption configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[GetBucketEncryptionArgs](http://minio.github.io/minio-java/io/minio/GetBucketEncryptionArgs.html)

Arguments.

Returns

[SseConfiguration](http://minio.github.io/minio-java/io/minio/messages/SseConfiguration.html) - Server-side encryption configuration.

Example

```
SseConfiguration config =
    minioClient.getBucketEncryption(
        GetBucketEncryptionArgs.builder().bucket("my-bucketname").build());
```

### getBucketLifecycle(GetBucketLifecycleArgs args)

`public LifecycleConfiguration getBucketLifecycle(GetBucketLifecycleArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getBucketLifecycle-io.minio.GetBucketLifecycleArgs-)

Gets lifecycle configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[GetBucketLifecycleArgs](http://minio.github.io/minio-java/io/minio/GetBucketLifecycleArgs.html)

Arguments.

Returns

[LifecycleConfiguration](http://minio.github.io/minio-java/io/minio/messages/LifecycleConfiguration.html) - lifecycle configuration.

Example

```
LifecycleConfiguration config =
    minioClient.getBucketLifecycle(
	    GetBucketLifecycleArgs.builder().bucket("my-bucketname").build());
System.out.println("Lifecycle configuration: " + config);
```

### getBucketNotification(GetBucketNotificationArgs args)

`public NotificationConfiguration getBucketNotification(GetBucketNotificationArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getBucketNotification-io.minio.GetBucketNotificationArgs-)

Gets notification configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[GetBucketNotificationArgs](http://minio.github.io/minio-java/io/minio/GetBucketNotificationArgs.html)

Arguments.

Returns

[NotificationConfiguration](http://minio.github.io/minio-java/io/minio/messages/NotificationConfiguration.html) - Notification configuration.

Example

```
NotificationConfiguration config =
    minioClient.getBucketNotification(
	    GetBucketNotificationArgs.builder().bucket("my-bucketname").build());
```

### getBucketPolicy(GetBucketPolicyArgs args)

`public String getBucketPolicy(GetBucketPolicyArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getBucketPolicy-io.minio.GetBucketPolicyArgs-)

Gets bucket policy configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[GetBucketPolicyArgs](http://minio.github.io/minio-java/io/minio/GetBucketPolicyArgs.html)

Arguments.

Returns

String - Bucket policy configuration as JSON string.

Example

```
String config =
    minioClient.getBucketPolicy(GetBucketPolicyArgs.builder().bucket("my-bucketname").build());
```

### getBucketReplication(GetBucketReplicationArgs args)

`public ReplicationConfiguration getBucketReplication(GetBucketReplicationArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getBucketReplication-io.minio.GetBucketReplicationArgs-)

Gets bucket replication configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[GetBucketReplicationArgs](http://minio.github.io/minio-java/io/minio/GetBucketReplicationArgs.html)

Arguments.

Returns

[ReplicationConfiguration](http://minio.github.io/minio-java/io/minio/messages/ReplicationConfiguration.html) - Bucket replication configuration.

Example

```
ReplicationConfiguration config =
    minioClient.getBucketReplication(
	    GetBucketReplicationArgs.builder().bucket("my-bucketname").build());
```

### getBucketTags(GetBucketTagsArgs args)

`public Tags getBucketTags(GetBucketTagsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.htmlgetBucketTags-io.minio.GetBucketTagsArgs-)

Gets tags of a bucket.

Parameters

Parameter

Type

Description

`args`

[GetBucketTagsArgs](http://minio.github.io/minio-java/io/minio/GetBucketTagsArgs.html)

Arguments.

Returns

[Tags](http://minio.github.io/minio-java/io/minio/messages/Tags.html) - tags.

Example

```
Tags tags = minioClient.getBucketTags(GetBucketTagsArgs.builder().bucket("my-bucketname").build());
```

### getBucketVersioning(GetBucketVersioningArgs args)

`public VersioningConfiguration getBucketVersioning(GetBucketVersioningArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getBucketVersioning-io.minio.GetBucketVersioningArgs-)

Gets versioning configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[GetBucketVersioningArgs](http://minio.github.io/minio-java/io/minio/GetBucketVersioningArgs.html)

Arguments.

Returns

[VersioningConfiguration](http://minio.github.io/minio-java/io/minio/messages/VersioningConfiguration.html) - Versioning configuration.

Example

```
VersioningConfiguration config =
    minioClient.getBucketVersioning(
        GetBucketVersioningArgs.builder().bucket("my-bucketname").build());
```

### getObjectLockConfiguration(GetObjectLockConfigurationArgs args)

`public ObjectLockConfiguration getObjectLockConfiguration(GetObjectLockConfigurationArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObjectLockConfiguration-io.minio.GetObjectLockConfigurationArgs-)

Gets object-lock configuration in a bucket.

Parameters

Parameter

Type

Description

`args`

[GetObjectLockConfigurationArgs](http://minio.github.io/minio-java/io/minio/GetObjectLockConfigurationArgs.html)

Arguments.

Returns

[ObjectLockConfiguration](http://minio.github.io/minio-java/io/minio/messages/ObjectLockConfiguration.html) - Default retention configuration.

Example

```
ObjectLockConfiguration config =
    minioClient.getObjectLockConfiguration(
	    GetObjectLockConfigurationArgs.builder().bucket("my-bucketname").build());
System.out.println("Mode: " + config.mode());
System.out.println("Duration: " + config.duration().duration() + " " + config.duration().unit());
```

### listBuckets()

`public List<Bucket> listBuckets()` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#listBuckets--)

Lists bucket information of all buckets.

Returns

List<[Bucket](http://minio.github.io/minio-java/io/minio/messages/Bucket.html)> - List of bucket information.

Example

```
List<Bucket> bucketList = minioClient.listBuckets();
for (Bucket bucket : bucketList) {
  System.out.println(bucket.creationDate() + ", " + bucket.name());
}
```

### listBuckets(ListBucketsArgs args)

`public List<Bucket> listBuckets(ListBucketsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#listBuckets-io.minio.ListBucketsArgs-)

Lists bucket information of all buckets.

Parameters

Parameter

Type

Description

`args`

[ListBucketsArgs](http://minio.github.io/minio-java/io/minio/ListBucketsArgs.html)

Arguments.

Returns

List<[Bucket](http://minio.github.io/minio-java/io/minio/messages/Bucket.html)> - List of bucket information.

Example

```
List<Bucket> bucketList =
    minioClient.listBuckets(ListBuckets.builder().extraHeaders(headers).build());
for (Bucket bucket : bucketList) {
  System.out.println(bucket.creationDate() + ", " + bucket.name());
}
```

### listenBucketNotification(ListenBucketNotificationArgs args)

`public CloseableIterator<Result<NotificationRecords>> listenBucketNotification(ListenBucketNotificationArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#listenBucketNotification-io.minio.ListenBucketNotificationArgs-)

Listens events of object prefix and suffix of a bucket. The returned closable iterator is lazily evaluated hence its required to iterate to get new records and must be used with try-with-resource to release underneath network resources.

Parameters

Parameter

Type

Description

`args`

[ListenBucketNotificationArgs](http://minio.github.io/minio-java/io/minio/ListenBucketNotificationArgs.html)

Arguments.

Returns

[CloseableIterator](http://minio.github.io/minio-java/io/minio/CloseableIterator.html)<[Result](http://minio.github.io/minio-java/io/minio/Result.html)<[NotificationRecords](http://minio.github.io/minio-java/io/minio/messages/NotificationRecords.html)>> - Lazy closable iterator contains event records.

Example

```
String[] events = {"s3:ObjectCreated:*", "s3:ObjectAccessed:*"};
try (CloseableIterator<Result<NotificationRecords>> ci =
    minioClient.listenBucketNotification(
        ListenBucketNotificationArgs.builder()
            .bucket("bucketName")
            .prefix("")
            .suffix("")
            .events(events)
            .build())) {
  while (ci.hasNext()) {
    NotificationRecords records = ci.next().get();
    for (Event event : records.events()) {
      System.out.println("Event " + event.eventType() + " occurred at " + event.eventTime()
          + " for " + event.bucketName() + "/" + event.objectName());
    }
  }
}
```

### listObjects(ListObjectsArgs args)

`public Iterable<Result<Item>> listObjects(ListObjectsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#listObjects-io.minio.ListObjectsArgs-)

Lists object information of a bucket.

Parameters

Parameter

Type

Description

`args`

[ListObjectsArgs](http://minio.github.io/minio-java/io/minio/ListObjectsArgs.html)

Arguments to list objects

Returns

Iterable<[Result](http://minio.github.io/minio-java/io/minio/Result.html)<[Item](http://minio.github.io/minio-java/io/minio/messages/Item.html)>> - Lazy iterator contains object information.

Example

```
// Lists objects information.
Iterable<Result<Item>> results = minioClient.listObjects(
    ListObjectsArgs.builder().bucket("my-bucketname").build());

// Lists objects information recursively.
Iterable<Result<Item>> results = minioClient.listObjects(
    ListObjectsArgs.builder().bucket("my-bucketname").recursive(true).build());

// Lists maximum 100 objects information whose names starts with 'E' and after 'ExampleGuide.pdf'.
Iterable<Result<Item>> results = minioClient.listObjects(
    ListObjectsArgs.builder()
        .bucket("my-bucketname")
        .startAfter("ExampleGuide.pdf")
        .prefix("E")
        .maxKeys(100)
        .build());

// Lists maximum 100 objects information with version whose names starts with 'E' and after
// 'ExampleGuide.pdf'.
Iterable<Result<Item>> results = minioClient.listObjects(
    ListObjectsArgs.builder()
        .bucket("my-bucketname")
        .startAfter("ExampleGuide.pdf")
        .prefix("E")
        .maxKeys(100)
        .includeVersions(true)
        .build());
```

### makeBucket(MakeBucketArgs args)

`public void makeBucket(MakeBucketArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#makeBucket-io.minio.MakeBucketArgs-)

Creates a bucket with given region and object lock feature enabled.

Parameters

Parameter

Type

Description

`args`

[MakeBucketArgs](http://minio.github.io/minio-java/io/minio/MakeBucketArgs.html)

Arguments to create bucket

Example

```
// Create bucket with default region.
minioClient.makeBucket(
    MakeBucketArgs.builder()
        .bucket("my-bucketname")
        .build());

// Create bucket with specific region.
minioClient.makeBucket(
    MakeBucketArgs.builder()
        .bucket("my-bucketname")
        .region("us-west-1")
        .build());

// Create object-lock enabled bucket with specific region.
minioClient.makeBucket(
    MakeBucketArgs.builder()
        .bucket("my-bucketname")
        .region("us-west-1")
        .objectLock(true)
        .build());
```

### removeBucket(RemoveBucketArgs args)

`public void removeBucket(RemoveBucketArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#removeBucket-io.minio.RemoveBucketArgs-)

Removes an empty bucket.

Parameters

Parameter

Type

Description

`args`

[RemoveBucketArgs](http://minio.github.io/minio-java/io/minio/RemoveBucketArgs.html)

Arguments.

Example

```
minioClient.removeBucket(RemoveBucketArgs.builder().bucket(bucketName).build());
```

### setBucketCors(SetBucketCorsArgs args)

`public void setBucketCors(SetBucketCorsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketCors-io.minio.SetBucketCorsArgs-)

Sets CORS configuration to a bucket.

Parameters

Parameter

Type

Description

`args`

[SetBucketCorsArgs](http://minio.github.io/minio-java/io/minio/SetBucketCorsArgs.html)

Arguments.

Example

```
CORSConfiguration config =
    new CORSConfiguration(
        Arrays.asList(
            new CORSConfiguration.CORSRule[] {
              // Rule 1
              new CORSConfiguration.CORSRule(
                  Arrays.asList(new String[] {"*"}), // Allowed headers
                  Arrays.asList(new String[] {"PUT", "POST", "DELETE"}), // Allowed methods
                  Arrays.asList(new String[] {"http://www.example.com"}), // Allowed origins
                  Arrays.asList(
                      new String[] {"x-amz-server-side-encryption"}), // Expose headers
                  null, // ID
                  3000), // Maximum age seconds
              // Rule 2
              new CORSConfiguration.CORSRule(
                  null, // Allowed headers
                  Arrays.asList(new String[] {"GET"}), // Allowed methods
                  Arrays.asList(new String[] {"*"}), // Allowed origins
                  null, // Expose headers
                  null, // ID
                  null // Maximum age seconds
                  )
            }));
minioClient.setBucketCors(SetBucketCorsArgs.builder().bucket("my-bucketname").config(config).build());
```

### setBucketEncryption(SetBucketEncryptionArgs args)

`public void setBucketEncryption(SetBucketEncryptionArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketEncryption-io.minio.SetBucketEncryptionArgs-)

Sets encryption configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[SetBucketEncryptionArgs](http://minio.github.io/minio-java/io/minio/SetBucketEncryptionArgs.html)

Arguments.

Example

```
minioClient.setBucketEncryption(
    SetBucketEncryptionArgs.builder().bucket("my-bucketname").config(config).build());
```

### setBucketLifecycle(SetBucketLifecycleArgs args)

`public void setBucketLifecycle(SetBucketLifecycleArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketLifecycle-io.minio.SetBucketLifecycleArgs-)

Sets lifecycle configuration to a bucket.

Parameters

Parameter

Type

Description

`args`

[SetBucketLifecycleArgs](http://minio.github.io/minio-java/io/minio/SetBucketLifecycleArgs.html)

Arguments.

Example

```
List<LifecycleRule> rules = new LinkedList<>();
rules.add(
    new LifecycleRule(
        Status.ENABLED,
        null,
        null,
        new RuleFilter("documents/"),
        "rule1",
        null,
        null,
        new Transition((ZonedDateTime) null, 30, "GLACIER")));
rules.add(
    new LifecycleRule(
        Status.ENABLED,
        null,
        new Expiration((ZonedDateTime) null, 365, null),
        new RuleFilter("logs/"),
        "rule2",
        null,
        null,
        null));
LifecycleConfiguration config = new LifecycleConfiguration(rules);
minioClient.setBucketLifecycle(
    SetBucketLifecycleArgs.builder().bucket("my-bucketname").config(config).build());
```

### setBucketNotification(SetBucketNotificationArgs args)

`public void setBucketNotification(SetBucketNotificationArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketNotification-io.minio.SetBucketNotificationArgs-)

Sets notification configuration to a bucket.

Parameters

Parameter

Type

Description

`args`

[SetBucketNotificationArgs](http://minio.github.io/minio-java/io/minio/SetBucketNotificationArgs.html)

Arguments.

Example

```
List<EventType> eventList = new LinkedList<>();
eventList.add(EventType.OBJECT_CREATED_PUT);
eventList.add(EventType.OBJECT_CREATED_COPY);

QueueConfiguration queueConfiguration = new QueueConfiguration();
queueConfiguration.setQueue("arn:minio:sqs::1:webhook");
queueConfiguration.setEvents(eventList);
queueConfiguration.setPrefixRule("images");
queueConfiguration.setSuffixRule("pg");

List<QueueConfiguration> queueConfigurationList = new LinkedList<>();
queueConfigurationList.add(queueConfiguration);

NotificationConfiguration config = new NotificationConfiguration();
config.setQueueConfigurationList(queueConfigurationList);

minioClient.setBucketNotification(
    SetBucketNotificationArgs.builder().bucket("my-bucketname").config(config).build());
```

### setBucketPolicy(SetBucketPolicyArgs args)

`public void setBucketPolicy(SetBucketPolicyArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketPolicy-io.minio.SetBucketPolicyArgs-)

Sets bucket policy configuration to a bucket.

Parameters

Parameter

Type

Description

`args`

[SetBucketPolicyArgs](http://minio.github.io/minio-java/io/minio/SetBucketPolicyArgs.html)

Arguments.

Example

```
// Assume policyJson contains below JSON string;
// {
//     "Statement": [
//         {
//             "Action": [
//                 "s3:GetBucketLocation",
//                 "s3:ListBucket"
//             ],
//             "Effect": "Allow",
//             "Principal": "*",
//             "Resource": "arn:aws:s3:::my-bucketname"
//         },
//         {
//             "Action": "s3:GetObject",
//             "Effect": "Allow",
//             "Principal": "*",
//             "Resource": "arn:aws:s3:::my-bucketname/myobject*"
//         }
//     ],
//     "Version": "2012-10-17"
// }
//
minioClient.setBucketPolicy(
    SetBucketPolicyArgs.builder().bucket("my-bucketname").config(policyJson).build());
```

### setBucketReplication(SetBucketReplicationArgs args)

`public void setBucketReplication(SetBucketReplicationArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketReplication-io.minio.SetBucketReplicationArgs-)

Sets bucket replication configuration to a bucket.

Parameters

Parameter

Type

Description

`args`

[SetBucketReplicationArgs](http://minio.github.io/minio-java/io/minio/SetBucketReplicationArgs.html)

Arguments.

Example

```
Map<String, String> tags = new HashMap<>();
tags.put("key1", "value1");
tags.put("key2", "value2");

ReplicationRule rule =
    new ReplicationRule(
        new DeleteMarkerReplication(Status.DISABLED),
        new ReplicationDestination(
            null, null, "REPLACE-WITH-ACTUAL-DESTINATION-BUCKET-ARN", null, null, null, null),
        null,
        new RuleFilter(new AndOperator("TaxDocs", tags)),
        "rule1",
        null,
        1,
        null,
        Status.ENABLED);

List<ReplicationRule> rules = new LinkedList<>();
rules.add(rule);

ReplicationConfiguration config =
    new ReplicationConfiguration("REPLACE-WITH-ACTUAL-ROLE", rules);

minioClient.setBucketReplication(
    SetBucketReplicationArgs.builder().bucket("my-bucketname").config(config).build());
```

### setBucketTags(SetBucketTagsArgs args)

`public void setBucketTags(SetBucketTagsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketTags-io.minio.SetBucketTagsArgs-)

Sets tags to a bucket.

Parameters

Parameter

Type

Description

`args`

[SetBucketTagsArgs](http://minio.github.io/minio-java/io/minio/SetBucketTagsArgs.html)

Arguments.

Example

```
Map<String, String> map = new HashMap<>();
map.put("Project", "Project One");
map.put("User", "jsmith");
minioClient.setBucketTags(SetBucketTagsArgs.builder().bucket("my-bucketname").tags(map).build());
```

### setBucketVersioning(SetBucketVersioningArgs args)

`public void setBucketVersioning(SetBucketVersioningArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setBucketVersioning-io.minio.SetBucketVersioningArgs-)

Sets versioning configuration of a bucket.

Parameters

Parameter

Type

Description

`args`

[SetBucketVersioningArgs](http://minio.github.io/minio-java/io/minio/SetBucketVersioningArgs.html)

Arguments.

Example

```
minioClient.setBucketVersioning(
    SetBucketVersioningArgs.builder().bucket("my-bucketname").config(config).build());
```

### setObjectLockConfiguration(SetObjectLockConfigurationArgs args)

`public void setObjectLockConfiguration(SetObjectLockConfigurationArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setObjectLockConfiguration-io.minio.SetObjectLockConfigurationArgs-)

Sets object-lock configuration in a bucket.

Parameters

Parameter

Type

Description

`args`

[SetObjectLockConfigurationArgs](http://minio.github.io/minio-java/io/minio/SetObjectLockConfigurationArgs.html)

Arguments.

Example

```
ObjectLockConfiguration config =
    new ObjectLockConfiguration(RetentionMode.COMPLIANCE, new RetentionDurationDays(100));
minioClient.setObjectLockConfiguration(
    SetObjectLockConfigurationArgs.builder().bucket("my-bucketname").config(config).build());
```

## 3. Object operations

### composeObject(ComposeObjectArgs args)

`public ObjectWriteResponse composeObject(ComposeObjectArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#composeObject-io.minio.ComposeObjectArgs--)

Creates an object by combining data from different source objects using server-side copy.

Parameters

Param

Type

Description

`args`

[ComposeObjectArgs](http://minio.github.io/minio-java/io/minio/ComposeObjectArgs.html)

Arguments.

Returns

[ObjectWriteResponse](http://minio.github.io/minio-java/io/minio/ObjectWriteResponse.html) - Contains information of created object.

Example

```
List<ComposeSource> sourceObjectList = new ArrayList<ComposeSource>();
sourceObjectList.add(
 ComposeSource.builder().bucket("my-job-bucket").object("my-objectname-part-one").build());
sourceObjectList.add(
 ComposeSource.builder().bucket("my-job-bucket").object("my-objectname-part-two").build());
sourceObjectList.add(
 ComposeSource.builder().bucket("my-job-bucket").object("my-objectname-part-three").build());

// Create my-bucketname/my-objectname by combining source object list.
minioClient.composeObject(
 ComposeObjectArgs.builder()
     .bucket("my-bucketname")
     .object("my-objectname")
     .sources(sourceObjectList)
     .build());

// Create my-bucketname/my-objectname with user metadata by combining source object
// list.
Map<String, String> userMetadata = new HashMap<>();
userMetadata.put("My-Project", "Project One");
minioClient.composeObject(
   ComposeObjectArgs.builder()
     .bucket("my-bucketname")
     .object("my-objectname")
     .sources(sourceObjectList)
     .userMetadata(userMetadata)
     .build());

// Create my-bucketname/my-objectname with user metadata and server-side encryption
// by combining source object list.
minioClient.composeObject(
 ComposeObjectArgs.builder()
     .bucket("my-bucketname")
     .object("my-objectname")
     .sources(sourceObjectList)
     .userMetadata(userMetadata)
     .ssec(sse)
     .build());
```

### copyObject(CopyObjectArgs args)

`public ObjectWriteResponse copyObject(CopyObjectArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#copyObject-io.minio.CopyObjectArgs-)

Creates an object by server-side copying data from another object.

Parameters

Parameter

Type

Description

`args`

[CopyObjectArgs](http://minio.github.io/minio-java/io/minio/CopyObjectArgs.html)

Arguments.

Returns

[ObjectWriteResponse](http://minio.github.io/minio-java/io/minio/ObjectWriteResponse.html) - Contains information of created object.

Example

```
// Create object "my-objectname" in bucket "my-bucketname" by copying from object
// "my-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .source(
            CopySource.builder()
                .bucket("my-source-bucketname")
                .object("my-objectname")
                .build())
        .build());

// Create object "my-objectname" in bucket "my-bucketname" by copying from object
// "my-source-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .source(
            CopySource.builder()
                .bucket("my-source-bucketname")
                .object("my-source-objectname")
                .build())
        .build());

// Create object "my-objectname" in bucket "my-bucketname" with SSE-KMS server-side
// encryption by copying from object "my-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .source(
            CopySource.builder()
                .bucket("my-source-bucketname")
                .object("my-objectname")
                .build())
        .sse(sseKms) // Replace with actual key.
        .build());

// Create object "my-objectname" in bucket "my-bucketname" with SSE-S3 server-side
// encryption by copying from object "my-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .source(
            CopySource.builder()
                .bucket("my-source-bucketname")
                .object("my-objectname")
                .build())
        .sse(sseS3) // Replace with actual key.
        .build());

// Create object "my-objectname" in bucket "my-bucketname" with SSE-C server-side encryption
// by copying from object "my-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .source(
            CopySource.builder()
                .bucket("my-source-bucketname")
                .object("my-objectname")
                .build())
        .sse(ssec) // Replace with actual key.
        .build());

// Create object "my-objectname" in bucket "my-bucketname" by copying from SSE-C encrypted
// object "my-source-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .source(
            CopySource.builder()
                .bucket("my-source-bucketname")
                .object("my-source-objectname")
                .ssec(ssec) // Replace with actual key.
                .build())
        .build());

// Create object "my-objectname" in bucket "my-bucketname" with custom headers conditionally
// by copying from object "my-objectname" in bucket "my-source-bucketname".
minioClient.copyObject(
    CopyObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .source(
            CopySource.builder()
                .bucket("my-source-bucketname")
                .object("my-objectname")
                .matchETag(etag) // Replace with actual etag.
                .build())
        .headers(headers) // Replace with actual headers.
        .build());
```

### deleteObjectTags(DeleteObjectTagsArgs args)

`private void deleteObjectTags(DeleteObjectTagsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#deleteObjectTags-io.minio.DeleteObjectTagsArgs-)

Deletes tags of an object.

Parameters

Parameter

Type

Description

`args`

[DeleteObjectTagsArgs](http://minio.github.io/minio-java/io/minio/DeleteObjectTagsArgs.html)

Arguments.

Example

```
minioClient.deleteObjectTags(
    DeleteObjectArgs.builder().bucket("my-bucketname").object("my-objectname").build());
```

### disableObjectLegalHold(DisableObjectLegalHoldArgs args)

`public void disableObjectLegalHold(DisableObjectLegalHoldArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#disableObjectLegalHold-io.minio.DisableObjectLegalHoldArgs-)

Disables legal hold on an object.

Parameters

Parameter

Type

Description

`args`

[DisableObjectLegalHoldArgs](http://minio.github.io/minio-java/io/minio/DisableObjectLegalHoldArgs.html)

Arguments.

Example

```
// Disables legal hold on an object.
minioClient.disableObjectLegalHold(
    DisableObjectLegalHoldArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .build());
```

### enableObjectLegalHold(EnableObjectLegalHoldArgs args)

`public void enableObjectLegalHold(EnableObjectLegalHoldArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#enableObjectLegalHold-io.minio.EnableObjectLegalHoldArgs-)

Enables legal hold on an object.

Parameters

Parameter

Type

Description

`args`

[EnableObjectLegalHoldArgs](http://minio.github.io/minio-java/io/minio/EnableObjectLegalHoldArgs.html)

Arguments.

Example

```
// Disables legal hold on an object.
minioClient.enableObjectLegalHold(
   EnableObjectLegalHoldArgs.builder()
       .bucket("my-bucketname")
       .object("my-objectname")
       .build());
```

### getObject(GetObjectArgs args)

`public InputStream getObject(GetObjectArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObject-io.minio.GetObjectArgs-)

Gets data of an object. Returned `InputStream` must be closed after use to release network resources.

Parameters

Parameter

Type

Description

`args`

GetObjectArgs

Arguments.

Returns

InputStream - Contains object data.

Example

```
// get object given the bucket and object name
try (InputStream stream = minioClient.getObject(
  GetObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .build())) {
  // Read data from stream
}

// get object data from offset
try (InputStream stream = minioClient.getObject(
  GetObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .offset(1024L)
  .build())) {
  // Read data from stream
}

// get object data from offset to length
try (InputStream stream = minioClient.getObject(
  GetObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .offset(1024L)
  .length(4096L)
  .build())) {
  // Read data from stream
}

// get data of an SSE-C encrypted object
try (InputStream stream = minioClient.getObject(
  GetObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .ssec(ssec)
  .build())) {
  // Read data from stream
}

// get object data from offset to length of an SSE-C encrypted object
try (InputStream stream = minioClient.getObject(
  GetObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .offset(1024L)
  .length(4096L)
  .ssec(ssec)
  .build())) {
  // Read data from stream
}
```

### getObjectAcl(GetObjectAclArgs args)

`public Acl getObjectAcl(GetObjectAclArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObjectAcl-io.minio.GetObjectAclArgs-)

Gets tags of an object.

Parameters

Parameter

Type

Description

`args`

[GetObjectAclArgs](http://minio.github.io/minio-java/io/minio/GetObjectAclArgs.html)

Arguments.

Returns

[AccessControlPolicy](http://minio.github.io/minio-java/io/minio/messages/AccessControlPolicy.html) - Access control policy.

Example

```
AccessControlPolicy policy = minioClient.getObjectAcl(
    GetObjectAclArgs.builder().bucket("my-bucketname").object("my-objectname").build());
```

### getObjectAttributes(GetObjectAttributesArgs args)

`public GetObjectAttributesResponse getObjectAttributes(GetObjectAttributesArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObjectAttributes-io.minio.GetObjectAttributesArgs-)

Gets tags of an object.

Parameters

Parameter

Type

Description

`args`

[GetObjectAttributesArgs](http://minio.github.io/minio-java/io/minio/GetObjectAttributesArgs.html)

Arguments.

Returns

[GetObjectAttributesResponse](http://minio.github.io/minio-java/io/minio/GetObjectAttributesResponse.html) - Response.

Example

```
GetObjectAttributesResponse response =
    minioClient.getObjectAttributes(
        GetObjectAttributesArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectname")
            .objectAttributes(
                new String[] {
                  "ETag", "Checksum", "ObjectParts", "StorageClass", "ObjectSize"
                })
            .build());
```

### downloadObject(DownloadObjectArgs args)

`public void downloadObject(DownloadObjectArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObject-io.minio.DownloadObjectArgs-)

Downloads data of an object to file.

Parameters

Parameter

Type

Description

`args`

DownloadObjectArgs

Arguments.

Example

```
// Download object given the bucket, object name and output file name
minioClient.downloadObject(
  DownloadObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .filename("my-object-file")
  .build());

// Download server-side encrypted object in bucket to given file name
minioClient.downloadObject(
  DownloadObjectArgs.builder()
  .bucket("my-bucketname")
  .object("my-objectname")
  .ssec(ssec)
  .filename("my-object-file")
  .build());
```

### getObjectRetention(GetObjectRetentionArgs args)

`public Retention getObjectRetention(GetObjectRetentionArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObjectRetention-io.minio.GetObjectRetentionArgs-)

Gets retention configuration of an object.

Parameters

Parameter

Type

Description

`args`

[GetObjectRetentionArgs](http://minio.github.io/minio-java/io/minio/GetObjectRetentionArgs.html)

Arguments.

Returns

[Retention](http://minio.github.io/minio-java/io/minio/messages/Retention.html) - Object retention configuration.

Example

```
// Object with version id.
Retention retention =
   minioClient.getObjectRetention(
       GetObjectRetentionArgs.builder()
           .bucket("my-bucketname")
           .object("my-objectname")
           .versionId("object-version-id")
           .build());
System.out.println("mode: " + retention.mode() + "until: " + retention.retainUntilDate());
```

### getObjectTags(GetObjectTagsArgs args)

`public Tags getObjectTags(GetObjectTagsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getObjectTags-io.minio.GetObjectTagsArgs-)

Gets tags of an object.

Parameters

Parameter

Type

Description

`args`

[GetObjectTagsArgs](http://minio.github.io/minio-java/io/minio/GetObjectTagsArgs.html)

Arguments.

Returns

[Tags](http://minio.github.io/minio-java/io/minio/messages/Tags.html) - tags.

Example

```
Tags tags = minioClient.getObjectTags(
    GetObjectTagsArgs.builder().bucket("my-bucketname").object("my-objectname").build());
```

### getPresignedObjectUrl(GetPresignedObjectUrlArgs args)

`public String getPresignedObjectUrl(GetPresignedObjectUrlArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getPresignedObjectUrl-io.minio.GetPresignedObjectUrlArgs-)

Gets presigned URL of an object for HTTP method, expiry time and custom request parameters.

Parameters

Parameter

Type

Description

`args`

[GetPresignedObjectUrlArgs](http://minio.github.io/minio-java/io/minio/GetPresignedObjectUrlArgs.html)

Arguments.

Returns

String - URL string.

Example

```
// Get presigned URL string to download 'my-objectname' in 'my-bucketname'
// with an expiration of 2 hours.
//
// Additionally also add 'response-content-type' to dynamically set content-type
// for the server response.
Map<String, String> reqParams = new HashMap<String, String>();
reqParams.put("response-content-type", "application/json");

String url =
   minioClient.getPresignedObjectUrl(
       GetPresignedObjectUrlArgs.builder()
           .method(Method.GET)
           .bucket("my-bucketname")
           .object("my-objectname")
           .expiry(2, TimeUnit.HOURS)
           .extraQueryParams(reqParams)
           .build());
System.out.println(url);

// Get presigned URL string to upload 'my-objectname' in 'my-bucketname'
// with an expiration of 1 day.
String url =
   minioClient.getPresignedObjectUrl(
       GetPresignedObjectUrlArgs.builder()
           .method(Method.PUT)
           .bucket("my-bucketname")
           .object("my-objectname")
           .expiry(1, TimeUnit.DAYS)
           .build());
System.out.println(url);

// Get presigned URL string to lookup metadata for 'my-objectname' in 'my-bucketname'
// with an expiration of 2 hours.
//
// Additionally also add 'response-content-type' to dynamically set content-type
// for the server metadata response.
Map<String, String> reqParams = new HashMap<String, String>();
reqParams.put("response-content-type", "application/json");

String url =
   minioClient.getPresignedObjectUrl(
       GetPresignedObjectUrlArgs.builder()
           .method(Method.HEAD)
           .bucket("my-bucketname")
           .object("my-objectname")
           .expiry(2, TimeUnit.HOURS)
           .extraQueryParams(reqParams)
           .build());
System.out.println(url);
```

### isObjectLegalHoldEnabled(IsObjectLegalHoldEnabledArgs args)

`public boolean isObjectLegalHoldEnabled(IsObjectLegalHoldEnabledArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#isObjectLegalHoldEnabled-io.minio.IsObjectLegalHoldEnabledArgs-)

Returns true if legal hold is enabled on an object.

Parameters

Parameter

Type

Description

`args`

[IsObjectLegalHoldEnabledArgs](http://minio.github.io/minio-java/io/minio/IsObjectLegalHoldEnabledArgs.html)

Arguments.

Returns

boolean - True if legal hold is enabled.

Example

```
boolean status =
    s3Client.isObjectLegalHoldEnabled(
       IsObjectLegalHoldEnabledArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectname")
            .versionId("object-versionId")
            .build());
if (status) {
  System.out.println("Legal hold is on");
else {
  System.out.println("Legal hold is off");
}
```

### getPresignedPostFormData(PostPolicy policy)

`public Map<String,String> getPresignedPostFormData(PostPolicy policy)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#getPresignedPostFormData-io.minio.PostPolicy-)

Gets form-data of [PostPolicy](http://minio.github.io/minio-java/io/minio/PostPolicy.html) of an object to upload its data using POST method.

Parameters

Parameter

Type

Description

`policy`

[PostPolicy](http://minio.github.io/minio-java/io/minio/PostPolicy.html)

Post policy of an object.

Returns

Map<String, String> - Contains form-data to upload an object using POST method.

Example

```
// Create new post policy for 'my-bucketname' with 7 days expiry from now.
PostPolicy policy = new PostPolicy("my-bucketname", ZonedDateTime.now().plusDays(7));

// Add condition that 'key' (object name) equals to 'my-objectname'.
policy.addEqualsCondition("key", "my-objectname");

// Add condition that 'Content-Type' starts with 'image/'.
policy.addStartsWithCondition("Content-Type", "image/");

// Add condition that 'content-length-range' is between 64kiB to 10MiB.
policy.addContentLengthRangeCondition(64 * 1024, 10 * 1024 * 1024);

Map<String, String> formData = minioClient.getPresignedPostFormData(policy);

// Upload an image using POST object with form-data.
MultipartBody.Builder multipartBuilder = new MultipartBody.Builder();
multipartBuilder.setType(MultipartBody.FORM);
for (Map.Entry<String, String> entry : formData.entrySet()) {
  multipartBuilder.addFormDataPart(entry.getKey(), entry.getValue());
}
multipartBuilder.addFormDataPart("key", "my-objectname");
multipartBuilder.addFormDataPart("Content-Type", "image/png");

// "file" must be added at last.
multipartBuilder.addFormDataPart(
    "file", "my-objectname", RequestBody.create(new File("Pictures/avatar.png"), null));

Request request =
    new Request.Builder()
        .url("https://play.min.io/my-bucketname")
        .post(multipartBuilder.build())
        .build();
OkHttpClient httpClient = new OkHttpClient().newBuilder().build();
Response response = httpClient.newCall(request).execute();
if (response.isSuccessful()) {
  System.out.println("Pictures/avatar.png is uploaded successfully using POST object");
} else {
  System.out.println("Failed to upload Pictures/avatar.png");
}
```

### promptObject(PromptObjectArgs args)

`public ObjectWriteResponse promptObject(PromptObjectArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#promptObject-io.minio.PromptObjectArgs-)

Performs language model inference with the prompt and referenced object as context.

Parameters

Parameter

Type

Description

`args`

[PromptObjectArgs](http://minio.github.io/minio-java/io/minio/PromptObjectArgs.html)

Arguments.

Returns

[PromptObjectResponse](http://minio.github.io/minio-java/io/minio/PromptObjectResponse.html) - response.

### putObject(PutObjectArgs args)

`public ObjectWriteResponse putObject(PutObjectArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#putObject-io.minio.PutObjectArgs-)

Uploads given stream as object in bucket.

Parameters

Parameter

Type

Description

`args`

[PutObjectArgs](http://minio.github.io/minio-java/io/minio/PutObjectArgs.html)

Arguments.

Returns

[ObjectWriteResponse](http://minio.github.io/minio-java/io/minio/ObjectWriteResponse.html) - Contains information of created object.

Example

```
// Upload known sized input stream.
minioClient.putObject(
    PutObjectArgs.builder().bucket("my-bucketname").object("my-objectname").stream(
            inputStream, size, -1)
        .contentType("video/mp4")
        .build());

// Upload unknown sized input stream.
minioClient.putObject(
    PutObjectArgs.builder().bucket("my-bucketname").object("my-objectname").stream(
            inputStream, -1, 10485760)
        .contentType("video/mp4")
        .build());

// Create object ends with '/' (also called as folder or directory).
minioClient.putObject(
    PutObjectArgs.builder().bucket("my-bucketname").object("path/to/").stream(
            new ByteArrayInputStream(new byte[] {}), 0, -1)
        .build());

// Upload input stream with headers and user metadata.
Map<String, String> headers = new HashMap<>();
headers.put("X-Amz-Storage-Class", "REDUCED_REDUNDANCY");
Map<String, String> userMetadata = new HashMap<>();
userMetadata.put("My-Project", "Project One");
minioClient.putObject(
    PutObjectArgs.builder().bucket("my-bucketname").object("my-objectname").stream(
            inputStream, size, -1)
        .headers(headers)
        .userMetadata(userMetadata)
        .build());

// Upload input stream with server-side encryption.
minioClient.putObject(
    PutObjectArgs.builder().bucket("my-bucketname").object("my-objectname").stream(
            inputStream, size, -1)
        .sse(sse)
        .build());
```

### putObjectFanOut(PutObjectFanOutArgs args)

`public PutObjectFanOutResponse putObjectFanOut(PutObjectFanOutArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#putObjectFanOut-io.minio.PutObjectFanOutArgs-)

Uploads multiple objects with same content from single stream with optional metadata and tags.

Parameters

Parameter

Type

Description

`args`

[PutObjectFanOutArgs](http://minio.github.io/minio-java/io/minio/PutObjectFanOutArgs.html)

Arguments.

Returns

[PutObjectFanOutResponse](http://minio.github.io/minio-java/io/minio/PutObjectFanOutResponse.html) - response.

Example

```
Map<String, String> map = new HashMap<>();
map.put("Project", "Project One");
map.put("User", "jsmith");
PutObjectFanOutResponse response =
    minioClient.putObjectFanOut(
        PutObjectFanOutArgs.builder().bucket("my-bucketname").stream(
                new ByteArrayInputStream("somedata".getBytes(StandardCharsets.UTF_8)), 8)
            .entries(
                Arrays.asList(
                    new PutObjectFanOutEntry[] {
                      PutObjectFanOutEntry.builder().key("fan-out.0").build(),
                      PutObjectFanOutEntry.builder().key("fan-out.1").tags(map).build()
                    }))
            .build());
```

### uploadObject(UploadObjectArgs args)

`public void uploadObject(UploadObjectArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#uploadObject-io.minio.UploadObjectArgs-)

Uploads contents from a file as object in bucket.

Parameters

Parameter

Type

Description

`args`

[UploadObjectArgs](http://minio.github.io/minio-java/io/minio/UploadObjectArgs.html)

Arguments.

Example

```
// Upload an JSON file.
minioClient.uploadObject(
    UploadObjectArgs.builder()
        .bucket("my-bucketname").object("my-objectname").filename("person.json").build());

// Upload a video file.
minioClient.uploadObject(
    UploadObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .filename("my-video.avi")
        .contentType("video/mp4")
        .build());
```

### uploadSnowballObjects(UploadSnowballObjectsArgs args)

`public void uploadSnowballObjects(UploadSnowballObjectsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#uploadSnowballObjects-io.minio.UploadSnowballObjectsArgs-)

Uploads multiple objects in a single put call. It is done by creating intermediate TAR file optionally compressed which is uploaded to S3 service.

Parameters

Parameter

Type

Description

`args`

[UploadSnowballObjectsArgs](http://minio.github.io/minio-java/io/minio/UploadSnowballObjectsArgs.html)

Arguments.

Example

```
List<SnowballObject> objects = new ArrayList<SnowballObject>();
objects.add(
    new SnowballObject(
        "my-object-one",
        new ByteArrayInputStream("hello".getBytes(StandardCharsets.UTF_8)),
        5,
        null));
objects.add(
    new SnowballObject(
        "my-object-two",
        new ByteArrayInputStream("java".getBytes(StandardCharsets.UTF_8)),
        4,
        null));
minioClient.uploadSnowballObjects(
    UploadSnowballObjectsArgs.builder().bucket("my-bucketname").objects(objects).build());
```

### removeObject(RemoveObjectArgs args)

`public void removeObject(RemoveObjectArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#removeObject-io.minio.RemoveObjectArgs-)

Removes an object.

Parameters

Parameter

Type

Description

`args`

[RemoveObjectArgs](http://minio.github.io/minio-java/io/minio/RemoveObjectArgs.html)

Arguments.

Example

```
// Remove object.
minioClient.removeObject(
    RemoveObjectArgs.builder().bucket("my-bucketname").object("my-objectname").build());

// Remove versioned object.
minioClient.removeObject(
    RemoveObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-versioned-objectname")
        .versionId("my-versionid")
        .build());

// Remove versioned object bypassing Governance mode.
minioClient.removeObject(
    RemoveObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-versioned-objectname")
        .versionId("my-versionid")
        .bypassRetentionMode(true)
        .build());
```

### removeObjects(RemoveObjectsArgs args)

`public Iterable<Result<DeleteError>> removeObjects(RemoveObjectsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#removeObjects-io.minio.RemoveObjectsArgs-)

Removes multiple objects lazily. Its required to iterate the returned Iterable to perform removal.

Parameters

Parameter

Type

Description

`args`

[RemoveObjectsArgs](http://minio.github.io/minio-java/io/minio/RemoveObjectsArgs.html)

Arguments.

Returns

Iterable<[Result](http://minio.github.io/minio-java/io/minio/Result.html)<[DeleteError](http://minio.github.io/minio-java/io/minio/messages/DeleteError.html)>> - Lazy iterator contains object removal status.

Example

```
List<DeleteObject> objects = new LinkedList<>();
objects.add(new DeleteObject("my-objectname1"));
objects.add(new DeleteObject("my-objectname2"));
objects.add(new DeleteObject("my-objectname3"));
Iterable<Result<DeleteError>> results =
    minioClient.removeObjects(
        RemoveObjectsArgs.builder().bucket("my-bucketname").objects(objects).build());
for (Result<DeleteError> result : results) {
  DeleteError error = result.get();
  System.out.println(
      "Error in deleting object " + error.objectName() + "; " + error.message());
}
```

### restoreObject(RestoreObjectArgs args)

`public void restoreObject(RestoreObjectArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#restoreObject-io.minio.RestoreObjectArgs-)

Restores an object.

Parameters

Parameter

Type

Description

`args`

[RestoreObjectArgs](http://minio.github.io/minio-java/io/minio/RestoreObjectArgs.html)

Arguments.

Example

```
// Restore object.
minioClient.restoreObject(
    RestoreObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .request(new RestoreRequest(null, null, null, null, null, null))
        .build());

// Restore versioned object.
minioClient.restoreObject(
    RestoreObjectArgs.builder()
        .bucket("my-bucketname")
        .object("my-versioned-objectname")
        .versionId("my-versionid")
        .request(new RestoreRequest(null, null, null, null, null, null))
        .build());
```

### selectObjectContent(SelectObjectContentArgs args)

`public SelectResponseStream selectObjectContent(SelectObjectContentArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#selectObjectContent-io.minio.SelectObjectContentArgs-)

Selects content of a object by SQL expression.

Parameters

Parameter

Type

Description

`args`

[SelectObjectContentArgs](http://minio.github.io/minio-java/io/minio/SelectObjectContentArgs.html)

Arguments.

Returns

[SelectResponseStream](http://minio.github.io/minio-java/io/minio/SelectResponseStream.html) - Contains filtered records and progress.

Example

```
String sqlExpression = "select * from S3Object";
InputSerialization is = new InputSerialization(null, false, null, null, FileHeaderInfo.USE, null, null, null);
OutputSerialization os = new OutputSerialization(null, null, null, QuoteFields.ASNEEDED, null);
SelectResponseStream stream =
    minioClient.selectObjectContent(
        SelectObjectContentArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectName")
            .sqlExpression(sqlExpression)
            .inputSerialization(is)
            .outputSerialization(os)
            .requestProgress(true)
            .build());

byte[] buf = new byte[512];
int bytesRead = stream.read(buf, 0, buf.length);
System.out.println(new String(buf, 0, bytesRead, StandardCharsets.UTF_8));

Stats stats = stream.stats();
System.out.println("bytes scanned: " + stats.bytesScanned());
System.out.println("bytes processed: " + stats.bytesProcessed());
System.out.println("bytes returned: " + stats.bytesReturned());

stream.close();
```

### setObjectRetention(SetObjectRetentionArgs args)

`public void setObjectLockRetention(SetObjectRetentionArgs)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setObjectRetention-io.minio.SetObjectRetentionArgs-)

Sets retention configuration to an object.

Parameters

Parameter

Type

Description

`args`

[SetObjectRetentionArgs](http://minio.github.io/minio-java/io/minio/SetObjectRetentionArgs.html)

Arguments.

Example

```
Retention retention = new Retention(RetentionMode.COMPLIANCE, ZonedDateTime.now().plusYears(1));
minioClient.setObjectRetention(
    SetObjectRetentionArgs.builder()
        .bucket("my-bucketname")
        .object("my-objectname")
        .config(retention)
        .bypassGovernanceMode(true)
        .build());
```

### setObjectTags(SetObjectTagsArgs args)

`public void setObjectTags(SetObjectTagsArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#setObjectTags-io.minio.SetObjectTagsArgs-)

Sets tags to an object.

Parameters

Parameter

Type

Description

`args`

[SetObjectTagsArgs](http://minio.github.io/minio-java/io/minio/SetObjectTagsArgs.html)

Arguments.

Example

```
Map<String, String> map = new HashMap<>();
map.put("Project", "Project One");
map.put("User", "jsmith");
minioClient.setObjectTags(
    SetObjectTagsArgs.builder().bucket("my-bucketname").object("my-objectname").tags(map).build());
```

### statObject(StatObjectArgs args)

`public StatObjectResponse statObject(StatObjectArgs args)` [[Javadoc]](http://minio.github.io/minio-java/io/minio/MinioClient.html#statObject-io.minio.StatObjectArgs-)

Gets object information and metadata of an object.

Parameters

Parameter

Type

Description

`args`

[StatObjectArgs](http://minio.github.io/minio-java/io/minio/StatObjectArgs.html)

Arguments.

Returns

[StatObjectResponse](http://minio.github.io/minio-java/io/minio/StatObjectResponse.html) - Populated object information and metadata.

Example

```
// Get information of an object.
StatObjectResponse response =
    minioClient.statObject(
        StatObjectArgs.builder().bucket("my-bucketname").object("my-objectname").build());

// Get information of SSE-C encrypted object.
StatObjectResponse response =
    minioClient.statObject(
        StatObjectArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectname")
            .ssec(ssec)
            .build());

// Get information of a versioned object.
StatObjectResponse response =
    minioClient.statObject(
        StatObjectArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectname")
            .versionId("version-id")
            .build());

// Get information of a SSE-C encrypted versioned object.
StatObjectResponse response =
    minioClient.statObject(
        StatObjectArgs.builder()
            .bucket("my-bucketname")
            .object("my-objectname")
            .versionId("version-id")
            .ssec(ssec)
            .build());
```

## 5. Explore Further

[Build your own Photo API Service - Full Application Example](https://github.com/minio/minio-java-rest-example)

[Complete JavaDoc](http://minio.github.io/minio-java/)

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).