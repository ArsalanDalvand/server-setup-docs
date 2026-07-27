---
title: ".NET Client API Reference — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/developers/dotnet/API.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "developers/dotnet/API"
category: "developers"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - ".NET Client API Reference — SILO (MinIO Object Storage)"
---
# .NET Client API Reference

## Initialize MinIO Client object.

## MinIO

```
IMinioClient minioClient = new MinioClient()
                              .WithEndpoint("play.min.io")
                              .WithCredentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
                              .WithSSL()
                              .Build();
```

## AWS S3

```
IIMinioClient minioClient = new MinioClient()
                              .WithEndpoint("s3.amazonaws.com")
                              .WithCredentials("YOUR-ACCESSKEYID", "YOUR-SECRETACCESSKEY")
                              .WithSSL()
                              .Build();
```

Bucket operations

Object operations

Presigned operations

Bucket Policy Operations

[`makeBucket`](#makeBucket)

[`getObject`](#getObject)

[`presignedGetObject`](#presignedGetObject)

[`getBucketPolicy`](#getBucketPolicy)

[`listBuckets`](#listBuckets)

[`putObject`](#putObject)

[`presignedPutObject`](#presignedPutObject)

[`setBucketPolicy`](#setBucketPolicy)

[`bucketExists`](#bucketExists)

[`copyObject`](#copyObject)

[`presignedPostPolicy`](#presignedPostPolicy)

[`setBucketNotification`](#setBucketNotification)

[`removeBucket`](#removeBucket)

[`statObject`](#statObject)

[`getBucketNotification`](#getBucketNotification)

[`listObjects`](#listObjects)

[`removeObject`](#removeObject)

[`removeAllBucketNotification`](#removeAllBucketNotification)

[`listIncompleteUploads`](#listIncompleteUploads)

[`removeObjects`](#removeObjects)

[`listenBucketNotifications`](#listenBucketNotifications)

[`selectObjectContent`](#selectObjectContent)

[`setVersioning`](#setVersioning)

[`setLegalHold`](#setLegalHold)

[`getVersioning`](#getVersioning)

[`getLegalHold`](#getLegalHold)

[`setBucketEncryption`](#setBucketEncryption)

[`setObjectTags`](#setObjectTags)

[`getBucketEncryption`](#getBucketEncryption)

[`getObjectTags`](#getObjectTags)

[`removeBucketEncryption`](#removeBucketEncryption)

[`removeObjectTags`](#removeObjectTags)

[`setBucketTags`](#setBucketTags)

[`setObjectRetention`](#setObjectRetention)

[`getBucketTags`](#getBucketTags)

[`getObjectRetention`](#getObjectRetention)

[`removeBucketTags`](#removeBucketTags)

[`clearObjectRetention`](#clearObjectRetention)

[`setObjectLock`](#setObjectLock)

[`removeIncompleteUpload`](#removeIncompleteUpload)

[`getObjectLock`](#getObjectLock)

[`removeObjectLock`](#removeObjectLock)

[`setBucketLifecycle`](#setBucketLifecycle)

[`getBucketLifecycle`](#getBucketLifecycle)

[`removeBucketLifecycle`](#removeBucketLifecycle)

[`setBucketReplication`](#setBucketReplication)

[`getBucketReplication`](#getBucketReplication)

[`removeBucketReplication`](#removeBucketReplication)

## 1. Constructors

`public MinioClient(string endpoint, string accessKey = "", string secretKey = "", string region = "", string sessionToken="")`

Creates MinIO client object with given endpoint.AccessKey, secretKey, region and sessionToken are optional parameters, and can be omitted for anonymous access.

The client object uses Http access by default. To use Https, chain method WithSSL() to client object to use secure transfer protocol

Parameters

Param

Type

Description

`endpoint`

string

endPoint is an URL, domain name, IPv4 address or IPv6 address.Valid endpoints are listed below:

s3.amazonaws.com

play.min.io

localhost

play.min.io

`accessKey`

string

accessKey is like user-id that uniquely identifies your account.This field is optional and can be omitted for anonymous access.

`secretKey`

string

secretKey is the password to your account.This field is optional and can be omitted for anonymous access.

`region`

string

region to which calls should be made.This field is optional and can be omitted.

`sessionToken`

string

sessionToken needs to be set if temporary access credentials are used

Secure Access (TLS)

`Chain .WithSSL() or WithSSL(true) to MinIO Client object to use https.`

`Chain .WithSSL(false) or nothing to Client object to use http.`

Proxy

`Chain .WithProxy(proxyObject) to MinIO Client object to use proxy`

`public MinioClient()`

Creates MinIO client. This client gives an empty object that can be used with Chaining to populate only the member variables we need.

The next important step is to connect to an endpoint. You can chain one of the overloaded method WithEndpoint() to client object to connect.

This client object also uses Http access by default. To use Https, chain method WithSSL() or WithSSL(true) to client object to use secure transfer protocol.

To use non-anonymous access, chain method WithCredentials() to the client object along with the access key & secret key.

Finally chain the method Build() to get the finally built client object.

Endpoint

`Chain .WithEndpoint() to MinIO Client object to initialize the endpoint.`

Return Type

Exceptions

`MinioClient`

Listed Exceptions:

Examples

### MinIO

```
// 1. Using Builder with public MinioClient(), Endpoint, Credentials & Secure (HTTPS) connection
IMinioClient minioClient = new MinioClient()
                              .WithEndpoint("play.min.io")
                              .WithCredentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
                              .WithSSL()
                              .Build()
// 2. Using Builder with public MinioClient(), Endpoint, Credentials & Secure (HTTPS) connection
IMinioClient minioClient = new MinioClient()
                              .WithEndpoint("play.min.io", 9000, true)
                              .WithCredentials("Q3AM3UQ867SPQQA43P2F", "zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG")
                              .WithSSL()
                              .Build()

// 3. Initializing minio client with proxy
IWebProxy proxy = new WebProxy("192.168.0.1", 8000);
IMinioClient minioClient = new MinioClient()
                              .WithEndpoint("my-ip-address:9000")
                              .WithCredentials("minio", "minio123")
                              .WithSSL()
                              .WithProxy(proxy)
                              .Build();
```

### AWS S3

```
// 1. Using Builder with public MinioClient(), Endpoint, Credentials, Secure (HTTPS) connection & proxy
IMinioClient s3Client = new MinioClient()
                           .WithEndpoint("s3.amazonaws.com")
                           .WithCredentials("YOUR-AWS-ACCESSKEYID", "YOUR-AWS-SECRETACCESSKEY")
                           .WithSSL()
                           .WithProxy(proxy)
                           .Build();
```

## 2. Bucket operations

### MakeBucketAsync(string bucketName, string location = “us-east-1”)

`Task MakeBucketAsync(string bucketName, string location = "us-east-1", CancellationToken cancellationToken = default(CancellationToken))`

Creates a new bucket.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket

`region`

string

Optional parameter. Defaults to us-east-1 for AWS requests

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`AccessDeniedException` : upon access denial

`RedirectionException` : upon redirection by server

`InternalClientException` : upon internal library error

Example

```
try
{
   // Create bucket if it doesn't exist.
   bool found = await minioClient.BucketExistsAsync("mybucket");
   if (found)
   {
      Console.WriteLine("mybucket already exists");
   }
   else
   {
     // Create bucket 'my-bucketname'.
     await minioClient.MakeBucketAsync("mybucket");
     Console.WriteLine("mybucket is created successfully");
   }
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### MakeBucketAsync(MakeBucketArgs args)

`Task MakeBucketAsync(MakeBucketArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Creates a new bucket.

Parameters

Param

Type

Description

`args`

MakeBucketArgs

Arguments Object - name, location.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`AccessDeniedException` : upon access denial

`RedirectionException` : upon redirection by server

`InternalClientException` : upon internal library error

Example

```
try
{
   // Create bucket if it doesn't exist.
   bool found = await minioClient.BucketExistsAsync(bktExistArgs);
   if (found)
   {
      Console.WriteLine(bktExistArgs.BucketName +" already exists");
   }
   else
   {
     // Create bucket 'my-bucketname'.
     await minioClient.MakeBucketAsync(mkBktArgs);
     Console.WriteLine(mkBktArgs.BucketName + " is created successfully");
   }
}
catch (MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### ListBucketsAsync()

`Task<ListAllMyBucketsResult> ListBucketsAsync(CancellationToken cancellationToken = default(CancellationToken))`

Lists all buckets.

Param

Type

Description

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<ListAllMyBucketsResult>` : Task with List of bucket type.

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`AccessDeniedException` : upon access denial

`InvalidOperationException`: upon unsuccessful deserialization of xml data

`ErrorResponseException` : upon unsuccessful execution

`InternalClientException` : upon internal library error

Example

```
try
{
    // List buckets that have read access.
    var list = await minioClient.ListBucketsAsync();
    foreach (Bucket bucket in list.Buckets)
    {
        Console.WriteLine(bucket.Name + " " + bucket.CreationDateDateTime);
    }
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### BucketExistsAsync(string bucketName)

`Task<bool> BucketExistsAsync(string bucketName, CancellationToken cancellationToken = default(CancellationToken))`

Checks if a bucket exists.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<bool>` : true if the bucket exists

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`AccessDeniedException` : upon access denial

`ErrorResponseException` : upon unsuccessful execution

`InternalClientException` : upon internal library error

Example

```
try
{
   // Check whether 'my-bucketname' exists or not.
   bool found = await minioClient.BucketExistsAsync(bucketName);
   Console.WriteLine("bucket-name " + ((found == true) ? "exists" : "does not exist"));
}
catch (MinioException e)
{
   Console.WriteLine("[Bucket]  Exception: {0}", e);
}
```

### BucketExistsAsync(BucketExistsArgs)

`Task<bool> BucketExistsAsync(BucketExistsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Checks if a bucket exists.

Parameters

Param

Type

Description

`args`

BucketExistsArgs

Argument object - bucket name.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<bool>` : true if the bucket exists

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`AccessDeniedException` : upon access denial

`ErrorResponseException` : upon unsuccessful execution

`InternalClientException` : upon internal library error

Example

```
try
{
   // Check whether 'my-bucketname' exists or not.
   bool found = await minioClient.BucketExistsAsync(args);
   Console.WriteLine(args.BucketName + " " + ((found == true) ? "exists" : "does not exist"));
}
catch (MinioException e)
{
   Console.WriteLine("[Bucket]  Exception: {0}", e);
}
```

### RemoveBucketAsync(string bucketName)

`Task RemoveBucketAsync(string bucketName, CancellationToken cancellationToken = default(CancellationToken))`

Removes a bucket.

NOTE: -  removeBucket does not delete the objects inside the bucket. The objects need to be deleted using the removeObject API.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

Task

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`AccessDeniedException` : upon access denial

`ErrorResponseException` : upon unsuccessful execution

`InternalClientException` : upon internal library error

`BucketNotFoundException` : upon missing bucket

Example

```
try
{
    // Check if my-bucket exists before removing it.
    bool found = await minioClient.BucketExistsAsync("mybucket");
    if (found)
    {
        // Remove bucket my-bucketname. This operation will succeed only if the bucket is empty.
        await minioClient.RemoveBucketAsync("mybucket");
        Console.WriteLine("mybucket is removed successfully");
    }
    else
    {
        Console.WriteLine("mybucket does not exist");
    }
}
catch(MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### RemoveBucketAsync(RemoveBucketArgs args)

`Task RemoveBucketAsync(RemoveBucketArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Removes a bucket.

NOTE: -  removeBucket does not delete the objects inside the bucket. The objects need to be deleted using the removeObject API.

Parameters

Param

Type

Description

`args`

RemoveBucketArgs

Arguments Object - bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

Task

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`AccessDeniedException` : upon access denial

`ErrorResponseException` : upon unsuccessful execution

`InternalClientException` : upon internal library error

`BucketNotFoundException` : upon missing bucket

Example

```
try
{
    // Check if my-bucket exists before removing it.
    bool found = await minioClient.BucketExistsAsync(bktExistsArgs);
    if (found)
    {
        // Remove bucket my-bucketname. This operation will succeed only if the bucket is empty.
        await minioClient.RemoveBucketAsync(rmBktArgs);
        Console.WriteLine(rmBktArgs.BucketName + " is removed successfully");
    }
    else
    {
        Console.WriteLine(bktExistsArgs.BucketName + " does not exist");
    }
}
catch(MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### public async Task GetVersioningAsync(GetVersioningArgs args)

`Task<VersioningConfiguration> GetVersioningAsync(GetVersioningArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Get versioning information for a bucket.

Parameters

Param

Type

Description

`args`

GetVersioningArgs

Arguments Object - bucket name.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`VersioningConfiguration`:VersioningConfiguration with information populated from response.

None

Example

```
try
{
    // Check whether 'mybucket' exists or not.
    bool found = minioClient.BucketExistsAsync(bktExistsArgs);
    if (found)
    {
        var args = new GetVersioningArgs("mybucket")
                       .WithSSL();
        VersioningConfiguration vc = await minio.GetVersioningInfoAsync(args);
    }
    else
    {
        Console.WriteLine(bktExistsArgs.BucketName + " does not exist");
    }
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### public async Task SetVersioningAsync(SetVersioningArgs args)

`Task SetVersioningAsync(SetVersioningArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Set versioning to Enabled or Suspended for a bucket.

Parameters

Param

Type

Description

`args`

SetVersioningArgs

Arguments Object - bucket name, versioning status.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`:

None

Example

```
try
{
    // Check whether 'mybucket' exists or not.
    bool found = minioClient.BucketExistsAsync(bktExistsArgs);
    if (found)
    {
        var args = new SetVersioningArgs("mybucket")
                       .WithSSL()
                       .WithVersioningEnabled();

        await minio.SetVersioningAsync(setArgs);
    }
    else
    {
        Console.WriteLine(bktExistsArgs.BucketName + " does not exist");
    }
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### SetBucketEncryptionAsync(SetBucketEncryptionArgs args)

`Task SetBucketEncryptionAsync(SetBucketEncryptionArgs args, CancellationToken cancellationToken = default(CancellationToken));`

Sets the Bucket Encryption Configuration of a bucket.

Parameters

Param

Type

Description

`args`

SetBucketEncryptionArgs

SetBucketEncryptionArgs Argument Object with bucket, encryption configuration

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Set Encryption Configuration for the bucket
    SetBucketEncryptionArgs args = new SetBucketEncryptionArgs()
                                       .WithBucket(bucketName)
                                       .WithEncryptionConfig(config);
    await minio.SetBucketEncryptionAsync(args);
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### GetBucketEncryptionAsync(GetBucketEncryptionArgs args)

`Task<ServerSideEncryptionConfiguration> GetBucketEncryptionAsync(GetBucketEncryptionArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Gets the Bucket Encryption configuration of the bucket.

Parameters

Param

Type

Description

`args`

GetBucketEncryptionArgs

GetBucketEncryptionArgs Argument Object with bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<ServerSideEncryptionConfiguration>`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

ServerSideEncryptionConfiguration  object which contains the bucket encryption configuration.

Example

```
try
{
    // Get Bucket Encryption Configuration for the bucket
    var args = new GetBucketEncryptionArgs()
                   .WithBucket(bucketName);
    ServerSideEncryptionConfiguration config = await minio.GetBucketEncryptionAsync(args);
    Console.WriteLine($"Got encryption configuration for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### RemoveBucketEncryptionAsync(RemoveBucketEncryptionArgs args)

`Task RemoveBucketEncryptionAsync(RemoveBucketEncryptionArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Remove the Bucket Encryption configuration of an object.

Parameters

Param

Type

Description

`args`

RemoveBucketEncryptionArgs

RemoveBucketEncryptionArgs Argument Object with bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Remove Bucket Encryption Configuration for the bucket
    var args = new RemoveBucketEncryptionArgs()
                   .WithBucket(bucketName);
    await minio.RemoveBucketEncryptionAsync(args);
    Console.WriteLine($"Removed encryption configuration for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### SetBucketTagsAsync(SetBucketTagsArgs args)

`Task SetBucketTagsAsync(SetBucketTagsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Sets tags to a bucket.

Parameters

Param

Type

Description

`args`

SetBucketTagsArgs

SetBucketTagsArgs Argument Object with bucket, tags to set

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Set Tags for the bucket
    SetBucketTagsArgs args = new SetBucketTagsArgs()
                                 .WithBucket(bucketName)
                                 .WithTagging(tags);
    await minio.SetBucketTagsAsync(args);
    Console.WriteLine($"Set Tags for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### GetBucketTagsAsync(GetBucketTagsArgs args)

`Task<Tagging> GetBucketTagsAsync(GetBucketTagsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Gets tags of a bucket.

Parameters

Param

Type

Description

`args`

GetBucketTagsArgs

GetBucketTagsArgs Argument Object with bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<Tagging>`: Tagging object which containing tag-value pairs.

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Get Bucket Tags for the bucket
    var args = new GetBucketTagsArgs()
                   .WithBucket(bucketName);
    var tags = await minio.GetBucketTagsAsync(args);
    Console.WriteLine($"Got tags for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### RemoveBucketTagsAsync(RemoveBucketTagsArgs args)

`Task RemoveBucketTagsAsync(RemoveBucketTagsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Deletes tags of a bucket.

Parameters

Param

Type

Description

`args`

RemoveBucketTagsArgs

RemoveBucketTagsArgs Argument Object with bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Remove Bucket Encryption Configuration for the bucket
    var args = new RemoveBucketTagsArgs()
                   .WithBucket(bucketName);
    await minio.RemoveBucketTagsAsync(args);
    Console.WriteLine($"Removed tags for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### SetBucketLifecycleAsync(SetBucketLifecycleArgs args)

`Task SetBucketLifecycleAsync(SetBucketLifecycleArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Sets Lifecycle configuration to a bucket.

Parameters

Param

Type

Description

`args`

SetBucketLifecycleArgs

SetBucketLifecycleArgs Argument Object with bucket name, Lifecycle configuration to set

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Set Lifecycle configuration for the bucket
    SetBucketLifecycleArgs args = new SetBucketLifecycleArgs()
                                      .WithBucket(bucketName)
                                      .WithConfiguration(lfc);
    await minio.SetBucketLifecycleAsync(args);
    Console.WriteLine($"Set Lifecycle for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### GetBucketLifecycleAsync(GetBucketLifecycleArgs args)

`Task<LifecycleConfiguration> GetBucketLifecycleAsync(GetBucketLifecycleArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Gets Lifecycle configuration of a bucket.

Parameters

Param

Type

Description

`args`

GetBucketLifecycleArgs

GetBucketLifecycleArgs Argument Object with bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<LifecycleConfiguration>`: LifecycleConfiguration object which contains the Lifecycle configuration details.

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Get Bucket Lifecycle configuration for the bucket
    var args = new GetBucketLifecycleArgs()
                   .WithBucket(bucketName);
    var lfc = await minio.GetBucketLifecycleAsync(args);
    Console.WriteLine($"Got Lifecycle configuration for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### RemoveBucketLifecycleAsync(RemoveBucketLifecycleArgs args)

`Task RemoveBucketLifecycleAsync(RemoveBucketLifecycleArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Deletes Lifecycle configuration of a bucket.

Parameters

Param

Type

Description

`args`

RemoveBucketLifecycleArgs

RemoveBucketLifecycleArgs Argument Object with bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Remove Bucket Lifecycle Configuration for the bucket
    var args = new RemoveBucketLifecycleArgs()
                   .WithBucket(bucketName);
    await minio.RemoveBucketLifecycleAsync(args);
    Console.WriteLine($"Removed Lifecycle configuration for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### SetBucketReplicationAsync(SetBucketReplicationArgs args)

`Task SetBucketReplicationAsync(SetBucketReplicationArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Sets Replication configuration to a bucket.

Parameters

Param

Type

Description

`args`

SetBucketReplicationArgs

SetBucketReplicationArgs Argument Object with bucket name, Replication configuration to set

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Set Replication configuration for the bucket
    SetBucketReplicationArgs args = new SetBucketReplicationArgs()
                                        .WithBucket(bucketName)
                                        .WithConfiguration(cfg);
    await minio.SetBucketReplicationAsync(args);
    Console.WriteLine($"Set Replication configuration for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### GetBucketReplicationAsync(GetBucketReplicationArgs args)

`Task<ReplicationConfiguration> GetBucketReplicationAsync(GetBucketReplicationArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Gets Replication configuration of a bucket.

Parameters

Param

Type

Description

`args`

GetBucketReplicationArgs

GetBucketReplicationArgs Argument Object with bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<ReplicationConfiguration>`: ReplicationConfiguration object which contains the Replication configuration details.

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Get Bucket Replication for the bucket
    var args = new GetBucketReplicationArgs()
                   .WithBucket(bucketName);
    var cfg = await minio.GetBucketReplicationAsync(args);
    Console.WriteLine($"Got Replication configuration for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### RemoveBucketReplicationAsync(RemoveBucketReplicationArgs args)

`Task RemoveBucketReplicationAsync(RemoveBucketReplicationArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Deletes Replication configuration of a bucket.

Parameters

Param

Type

Description

`args`

RemoveBucketReplicationArgs

RemoveBucketReplicationArgs Argument Object with bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Remove Bucket Replication Configuration for the bucket
    var args = new RemoveBucketReplicationArgs()
                   .WithBucket(bucketName);
    await minio.RemoveBucketReplicationAsync(args);
    Console.WriteLine($"Removed Replication configuration for bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### ListObjectsAsync(ListObjectArgs args)

`IObservable<Item> ListObjectsAsync(ListObjectArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Lists all objects (with version IDs, if existing) in a bucket.

Parameters

Param

Type

Description

`args`

ListObjectArgs

ListObjectArgs object - encapsulates bucket name, prefix, show recursively, show versions.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`IObservable<Item>`:an Observable of Items.

None

Example

```
try
{
    // Just list of objects
    // Check whether 'mybucket' exists or not.
    bool found = minioClient.BucketExistsAsync("mybucket");
    if (found)
    {
        // List objects from 'my-bucketname'
        ListObjectArgs args = new ListObjectArgs()
                                  .WithBucket("mybucket")
                                  .WithPrefix("prefix")
                                  .WithRecursive(true);
        IObservable<Item> observable = minioClient.ListObjectsAsync(args);
        IDisposable subscription = observable.Subscribe(
                item => Console.WriteLine("OnNext: {0}", item.Key),
                ex => Console.WriteLine("OnError: {0}", ex.Message),
                () => Console.WriteLine("OnComplete: {0}"));
    }
    else
    {
        Console.WriteLine("mybucket does not exist");
    }
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}

try
{
    // List of objects with version IDs.
    // Check whether 'mybucket' exists or not.
    bool found = minioClient.BucketExistsAsync("mybucket");
    if (found)
    {
        // List objects from 'my-bucketname'
        ListObjectArgs args = new ListObjectArgs()
                                  .WithBucket("mybucket")
                                  .WithPrefix("prefix")
                                  .WithRecursive(true)
                                  .WithVersions(true)
        IObservable<Item> observable = minioClient.ListObjectsAsync(args, true);
        IDisposable subscription = observable.Subscribe(
                item => Console.WriteLine("OnNext: {0} - {1}", item.Key, item.VersionId),
                ex => Console.WriteLine("OnError: {0}", ex.Message),
                () => Console.WriteLine("OnComplete: {0}"));
    }
    else
    {
        Console.WriteLine("mybucket does not exist");
    }
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### SetObjectLockConfigurationAsync(SetObjectLockConfigurationArgs args)

`Task SetObjectLockConfigurationAsync(SetObjectLockConfigurationArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Sets object-lock configuration in a bucket.

Parameters

Param

Type

Description

`args`

SetObjectLockConfigurationArgs

SetObjectLockConfigurationArgs Argument Object with bucket, lock configuration to set

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    ObjectLockConfiguration config = = new ObjectLockConfiguration(RetentionMode.GOVERNANCE, 35);
    // Set Object Lock Configuration for the bucket
    SetObjectLockConfigurationArgs args = new SetObjectLockConfigurationArgs()
                                              .WithBucket(bucketName)
                                              .WithLockConfiguration(config);
    await minio.SetObjectLockConfigurationAsync(args);
    Console.WriteLine($"Set Object lock configuration to bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### GetObjectLockConfigurationAsync(GetObjectLockConfigurationArgs args)

`Task<ObjectLockConfiguration> GetObjectLockConfigurationAsync(GetObjectLockConfigurationArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Gets object-lock configuration of a bucket.

Parameters

Param

Type

Description

`args`

GetObjectLockConfigurationArgs

GetObjectLockConfigurationArgs Argument Object with bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<ObjectLockConfiguration>`: ObjectLockConfiguration object which containing lock-enabled status & Object lock rule.

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Get the Object Lock Configuration for the bucket
    var args = new GetObjectLockConfigurationArgs()
                   .WithBucket(bucketName);
    var config = await minio.GetObjectLockConfigurationAsync(args);
    Console.WriteLine($"Object lock configuration on bucket {bucketName} is : " + config.ObjectLockEnabled);
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### RemoveObjectLockConfigurationAsync(RemoveObjectLockConfigurationArgs args)

`Task RemoveObjectLockConfigurationAsync(RemoveObjectLockConfigurationArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Removes object-lock configuration on a bucket.

Parameters

Param

Type

Description

`args`

RemoveObjectLockConfigurationArgs

RemoveObjectLockConfigurationArgs Argument Object with bucket name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`BucketNotFoundException` : upon bucket with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Remove Object Lock Configuration on the bucket
    var args = new RemoveObjectLockConfigurationArgs()
                   .WithBucket(bucketName);
    await minio.RemoveObjectLockConfigurationAsync(args);
    Console.WriteLine($"Removed Object lock configuration on bucket {bucketName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### ListIncompleteUploads(ListIncompleteUploadsArgs args)

`IObservable<Upload> ListIncompleteUploads(ListIncompleteUploadsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Lists partially uploaded objects in a bucket.

Parameters

Param

Type

Description

`args`

ListIncompleteUploadsArgs

ListIncompleteUploadsArgs object - encapsulates bucket name, prefix, show recursively.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`IObservable<Upload>`: an Observable of Upload.

None

Example

```
try
{
    // Check whether 'mybucket' exist or not.
    bool found = minioClient.BucketExistsAsync("mybucket");
    if (found)
    {
        // List all incomplete multipart upload of objects in 'mybucket'
        ListIncompleteUploadsArgs listArgs = new ListIncompleteUploadsArgs()
                                                 .WithBucket("mybucket")
                                                 .WithPrefix("prefix")
                                                 .WithRecursive(true);
        IObservable<Upload> observable = minioClient.ListIncompleteUploads(listArgs);
        IDisposable subscription = observable.Subscribe(
                            item => Console.WriteLine("OnNext: {0}", item.Key),
                            ex => Console.WriteLine("OnError: {0}", ex.Message),
                            () => Console.WriteLine("OnComplete: {0}"));
    }
    else
    {
        Console.WriteLine("mybucket does not exist");
    }
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### ListenBucketNotificationsAsync(ListenBucketNotificationsArgs args)

`IObservable<MinioNotificationRaw> ListenBucketNotificationsAsync(ListenBucketNotificationsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Subscribes to bucket change notifications (a Minio-only extension)

Parameters

Param

Type

Description

`args`

ListenBucketNotificationsArgs

ListenBucketNotificationsArgs object - encapsulates bucket name, list of events, prefix, suffix.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`IObservable<MinioNotificationRaw>`: an Observable of MinioNotificationRaw, which contain the raw JSON notifications. Use the MinioNotification class to deserialise using the JSON library of your choice.

None

Example

```
try
{
    var events = new List<EventType> { EventType.ObjectCreatedAll };
    var prefix = null;
    var suffix = null;
    ListenBucketNotificationsArgs args = new ListenBucketNotificationsArgs()
                                             .WithBucket(bucketName)
                                             .WithEvents(events)
                                             .WithPrefix(prefix)
                                             .WithSuffix(suffix);
    IObservable<MinioNotificationRaw> observable = minioClient.ListenBucketNotificationsAsync(args);

    IDisposable subscription = observable.Subscribe(
        notification => Console.WriteLine($"Notification: {notification.json}"),
        ex => Console.WriteLine($"OnError: {ex}"),
        () => Console.WriteLine($"Stopped listening for bucket notifications\n"));

}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### GetPolicyAsync(GetPolicyArgs args)

`Task<String> GetPolicyAsync(GetPolicyArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Get bucket policy.

Parameters

Param

Type

Description

`args`

GetPolicyArgs

GetPolicyArgs object encapsulating bucket name.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<String>`: The current bucket policy for given bucket as a json string.

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name.

`InvalidObjectPrefixException` : upon invalid object prefix.

`ConnectionException` : upon connection error.

`AccessDeniedException` : upon access denial

`InternalClientException` : upon internal library error.

`BucketNotFoundException` : upon missing bucket

Example

```
try
{
    GetPolicyArgs args = new GetPolicyArgs()
                             .WithBucket("myBucket");
    String policyJson = await minioClient.GetPolicyAsync(args);
    Console.WriteLine("Current policy: " + policyJson);
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### SetPolicyAsync(SetPolicyArgs args)

`Task SetPolicyAsync(SetPolicyArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Set policy on bucket.

Parameters

Param

Type

Description

`args`

SetPolicyArgs

SetPolicyArgs object encapsulating bucket name, Policy as a json string.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

Task

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`InternalClientException` : upon internal library error

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectPrefixException` : upon invalid object prefix

Example

```
try
{
    string policyJson = $@"{{""Version"":""2012-10-17"",""Statement"":[{{""Action"":[""s3:GetBucketLocation""],""Effect"":""Allow"",""Principal"":{{""AWS"":[""*""]}},""Resource"":[""arn:aws:s3:::{bucketName}""],""Sid"":""""}},{{""Action"":[""s3:ListBucket""],""Condition"":{{""StringEquals"":{{""s3:prefix"":[""foo"",""prefix/""]}}}},""Effect"":""Allow"",""Principal"":{{""AWS"":[""*""]}},""Resource"":[""arn:aws:s3:::{bucketName}""],""Sid"":""""}},{{""Action"":[""s3:GetObject""],""Effect"":""Allow"",""Principal"":{{""AWS"":[""*""]}},""Resource"":[""arn:aws:s3:::{bucketName}/foo*"",""arn:aws:s3:::{bucketName}/prefix/*""],""Sid"":""""}}]}}";
    SetPolicyArgs args = new SetPolicyArgs()
                             .WithBucket("myBucket")
                             .WithPolicy(policyJson);
    await minioClient.SetPolicyAsync(args);
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### SetBucketNotificationAsync(SetBucketNotificationsArgs args)

`Task SetBucketNotificationAsync(SetBucketNotificationsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Sets notification configuration for a given bucket

Parameters

Param

Type

Description

`args`

SetBucketNotificationsArgs

SetBucketNotificationsArgs object encapsulating bucket name, notification configuration object.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

Task

Listed Exceptions:

`ConnectionException` : upon connection error

`InternalClientException` : upon internal library error

`InvalidBucketNameException` : upon invalid bucket name

`InvalidOperationException`: upon unsuccessful serialization of notification object

Example

```
try
{
    BucketNotification notification = new BucketNotification();
    Arn topicArn = new Arn("aws", "sns", "us-west-1", "412334153608", "topicminio");

    TopicConfig topicConfiguration = new TopicConfig(topicArn);
    List<EventType> events = []{ EventType.ObjectCreatedPut , EventType.ObjectCreatedCopy };
    topicConfiguration.AddEvents(events);
    topicConfiguration.AddFilterPrefix("images");
    topicConfiguration.AddFilterSuffix("jpg");
    notification.AddTopic(topicConfiguration);

    QueueConfig queueConfiguration = new QueueConfig("arn:aws:sqs:us-west-1:482314153608:testminioqueue1");
    queueConfiguration.AddEvents([] { EventType.ObjectCreatedCompleteMultipartUpload });
    notification.AddQueue(queueConfiguration);

    SetBucketNotificationsArgs args = new SetBucketNotificationsArgs()
                                          .WithBucket(bucketName)
                                          .WithBucketNotificationConfiguration(notification);
    await minio.SetBucketNotificationsAsync(args);
    Console.WriteLine("Notifications set for the bucket " + args.BucketName + " successfully");
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### GetBucketNotificationAsync(GetBucketNotificationsArgs args)

`Task<BucketNotification> GetBucketNotificationAsync(GetBucketNotificationsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Get bucket notification configuration

Parameters

Param

Type

Description

`args`

GetBucketNotificationsArgs

GetBucketNotificationsArgs object encapsulating bucket name.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<BucketNotification>`: The current notification configuration for the bucket.

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name.

`ConnectionException` : upon connection error.

`AccessDeniedException` : upon access denial

`InternalClientException` : upon internal library error.

`BucketNotFoundException` : upon missing bucket

`InvalidOperationException`: upon unsuccessful deserialization of xml data

Example

```
try
{
    GetBucketNotificationsArgs args = new GetBucketNotificationsArgs()
                                          .WithBucket(bucketName);
    BucketNotification notifications = await minioClient.GetBucketNotificationAsync(args);
    Console.WriteLine("Notifications is " + notifications.ToXML());
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### RemoveAllBucketNotificationsAsync(RemoveAllBucketNotificationsArgs args)

`Task RemoveAllBucketNotificationsAsync(RemoveAllBucketNotificationsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Remove all notification configurations set on the bucket

Parameters

Param

Type

Description

`args`

RemoveAllBucketNotificationsArgs

RemoveAllBucketNotificationsArgs args encapsulating the bucket name.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

``Task`:

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name.

`ConnectionException` : upon connection error.

`AccessDeniedException` : upon access denial

`InternalClientException` : upon internal library error.

`BucketNotFoundException` : upon missing bucket

`InvalidOperationException`: upon unsuccessful serialization of xml data

Example

```
try
{
    RemoveAllBucketNotificationsArgs args = new RemoveAllBucketNotificationsArgs()
                                                .WithBucket(bucketName);
    await minioClient.RemoveAllBucketNotificationsAsync(args);
    Console.WriteLine("Notifications successfully removed from the bucket " + bucketName);
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

## 3. Object operations

### GetObjectAsync(GetObjectArgs args, ServerSideEncryption sse)

`Task GetObjectAsync(GetObjectArgs args, ServerSideEncryption sse = null, CancellationToken cancellationToken = default(CancellationToken))`

Downloads an object.

Parameters

Param

Type

Description

`args`

GetObjectArgs

GetObjectArgs Argument Object encapsulating bucket, object names, version Id, ServerSideEncryption object, offset, length

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`: Task callback returns an InputStream containing the object data.

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name.

`ConnectionException` : upon connection error.

`InternalClientException` : upon internal library error.

Examples

```
// 1. With Bucket & Object names.
try
{
   // Check whether the object exists using statObject().
   // If the object is not found, statObject() throws an exception,
   // else it means that the object exists.
   // Execution is successful.
   StatObjectArgs statObjectArgs = new StatObjectArgs()
                                       .WithBucket("mybucket")
                                       .WithObject("myobject");
   await minioClient.StatObjectAsync(statObjectArgs);

   // Get input stream to have content of 'my-objectname' from 'my-bucketname'
   GetObjectArgs getObjectArgs = new GetObjectArgs()
                                     .WithBucket("mybucket")
                                     .WithObject("myobject")
                                     .WithCallbackStream((stream) =>
                                          {
                                              stream.CopyTo(Console.OpenStandardOutput());
                                          });
   await minioClient.GetObjectAsync(getObjectArgs);
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}

// 2. With Offset Length specifying a range of bytes & the object as a stream.
try
{
    // Check whether the object exists using statObject().
    // If the object is not found, statObject() throws an exception,
    // else it means that the object exists.
    // Execution is successful.
    StatObjectArgs statObjectArgs = new StatObjectArgs()
                                        .WithBucket("mybucket")
                                        .WithObject("myobject");
    await minioClient.StatObjectAsync(statObjectArgs);

    // Get input stream to have content of 'my-objectname' from 'my-bucketname'
    GetObjectArgs getObjectArgs = new GetObjectArgs()
                                      .WithBucket("mybucket")
                                      .WithObject("myobject")
                                      .WithOffset(1024L)
                                      .WithObjectSize(10L)
                                      .WithCallbackStream((stream) =>
    {
        stream.CopyTo(Console.OpenStandardOutput());
    });
    await minioClient.GetObjectAsync(getObjectArgs);
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}

// 3. Downloads and saves the object as a file in the local filesystem.
try
{
    // Check whether the object exists using statObjectAsync().
    // If the object is not found, statObjectAsync() throws an exception,
    // else it means that the object exists.
    // Execution is successful.
    StatObjectArgs statObjectArgs = new StatObjectArgs()
                                        .WithBucket("mybucket")
                                        .WithObject("myobject");
    await minioClient.StatObjectAsync(statObjectArgs);

    // Gets the object's data and stores it in photo.jpg
    GetObjectArgs getObjectArgs = new GetObjectArgs()
                                      .WithBucket("mybucket")
                                      .WithObject("myobject")
                                      .WithFileName("photo.jpg");
    await minioClient.GetObjectAsync(getObjectArgs);
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### PutObjectAsync(PutObjectArgs args)

`Task PutObjectAsync(PutObjectArgs args, CancellationToken cancellationToken = default(CancellationToken))`

PutObjectAsync: Uploads object from a file or stream.

Parameters

Param

Type

Description

`args`

PutObjectArgs

Arguments object - bucket name, object name, file name, object data stream, object size, content type, object metadata, operation executing progress, SSE. etc.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectNameException` : upon invalid object name

`ConnectionException` : upon connection error

`InternalClientException` : upon internal library error

`EntityTooLargeException`: upon proposed upload size exceeding max allowed

`UnexpectedShortReadException`: data read was shorter than size of input buffer

`ArgumentNullException`: upon null input stream

`InvalidOperationException`: upon input value to PutObjectArgs being invalid

Example

The maximum size of a single object is limited to 5TB. putObject transparently uploads objects larger than 5MiB in multiple parts. Uploaded data is carefully verified using MD5SUM signatures.

```
try
{
    Aes aesEncryption = Aes.Create();
    aesEncryption.KeySize = 256;
    aesEncryption.GenerateKey();
    var ssec = new SSEC(aesEncryption.Key);
    var progress = new Progress<ProgressReport>(progressReport =>
    {
        // Progress events are delivered asynchronously (see remark below)
        Console.WriteLine(
                $"Percentage: {progressReport.Percentage}% TotalBytesTransferred: {progressReport.TotalBytesTransferred} bytes");
        if (progressReport.Percentage != 100)
            Console.SetCursorPosition(0, Console.CursorTop - 1);
        else Console.WriteLine();
    });
    PutObjectArgs putObjectArgs = new PutObjectArgs()
                                      .WithBucket("mybucket")
                                      .WithObject("island.jpg")
                                      .WithFilename("/mnt/photos/island.jpg")
                                      .WithContentType("application/octet-stream")
                                      .WithServerSideEncryption(ssec)
                                      .WithProgress(progress);
    await minio.PutObjectAsync(putObjectArgs);
    Console.WriteLine("island.jpg is uploaded successfully");
}
catch(MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

Remark:
Note that the default [Progress class](https://learn.microsoft.com/en-us/dotnet/api/System.Progress-1) post progress
updates to the SynchronizationContext when the instance was constructed
([source](https://learn.microsoft.com/en-us/dotnet/api/system.progress-1#remarks)). This means that progress updates are
sent asynchronous. If you do want to receive the events synchronous, then use the `Minio.Helper.SyncProgress<T>` instead,
but make sure you understand the implications. Progress events are sent synchronous, so they may be invoked on an
arbitrary thread (mostly a problem for UI applications) and performing long-running and/or blocking operations will
degrade upload performance.

### StatObjectAsync(StatObjectArgs args)

`Task<ObjectStat> StatObjectAsync(StatObjectArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Gets metadata of an object.

Parameters

Param

Type

Description

`args`

StatObjectArgs

StatObjectArgs Argument Object with bucket, object names & server side encryption object

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<ObjectStat>`: Populated object meta data.

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`InternalClientException` : upon internal library error

Example

```
try
{
   // Get the metadata of the object.
   StatObjectArgs statObjectArgs = new StatObjectArgs()
                                       .WithBucket("mybucket")
                                       .WithObject("myobject");
   ObjectStat objectStat = await minioClient.StatObjectAsync(statObjectArgs);
   Console.WriteLine(objectStat);
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### CopyObjectAsync(CopyObjectArgs args)

`Task<CopyObjectResult> CopyObjectAsync(CopyObjectArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Copies content from objectName to destObjectName.

Parameters

Param

Type

Description

`args`

CopyObjectArgs

Arguments object - bucket name, object name, destination bucket name, destination object name, copy conditions, metadata, Source SSE, Destination SSE. etc.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectNameException` : upon invalid object name

`BucketNotFoundException` : upon bucket with name not found

`ObjectNotFoundException` : upon object with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`ConnectionException` : upon connection error

`InternalClientException` : upon internal library error

`ArgumentException` : upon missing bucket/object names

Example

This API performs a Server-side copy operation from a given source object to destination object.

```
try
{
   CopyConditions copyConditions = new CopyConditions();
   copyConditions.setMatchETagNone("TestETag");
   ServerSideEncryption sseSrc, sseDst;
   // Uncomment to specify source and destination Server-side encryption options
   /*
    Aes aesEncryption = Aes.Create();
    aesEncryption.KeySize = 256;
    aesEncryption.GenerateKey();
    sseSrc = new SSEC(aesEncryption.Key);
    sseDst = new SSES3();
   */
   await minioClient.CopyObjectAsync("mybucket",  "island.jpg", "mydestbucket", "processed.png", copyConditions, sseSrc:sseSrc, sseDest:sseDst);
   Console.WriteLine("island.jpg is uploaded successfully");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### RemoveObjectAsync(RemoveObjectArgs args)

`Task RemoveObjectAsync(RemoveObjectArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Removes an object.

Parameters

Param

Type

Description

`args`

RemoveObjectArgs

Arguments Object.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`InternalClientException` : upon internal library error

Example

```
// 1. Remove object myobject from the bucket mybucket.
try
{
    RemoveObjectArgs rmArgs = new RemoveObjectArgs()
                                  .WithBucket("mybucket")
                                  .WithObject("myobject");
    await minioClient.RemoveObjectAsync(args);
    Console.WriteLine("successfully removed mybucket/myobject");
}
catch (MinioException e)
{
    Console.WriteLine("Error: " + e);
}

// 2. Remove one version of object myobject with versionID from mybucket.
try
{
    RemoveObjectArgs rmArgs = new RemoveObjectArgs()
                                  .WithBucket("mybucket")
                                  .WithObject("myobject")
                                  .WithVersionId("versionId");
    await minioClient.RemoveObjectAsync(args);
    Console.WriteLine("successfully removed mybucket/myobject{versionId}");
}
catch (MinioException e)
{
    Console.WriteLine("Error: " + e);
}
```

### RemoveObjectsAsync(RemoveObjectsArgs args)

`Task<IObservable<DeleteError>> RemoveObjectsAsync(RemoveObjectsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Removes a list of objects or object versions.

Parameters

Param

Type

Description

`args`

RemoveObjectsArgs

Arguments Object - bucket name, List of Objects to be deleted or List of Tuples with Tuple(object name, List of version IDs).

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`InternalClientException` : upon internal library error

Example

```
// 1. Remove list of objects in objectNames from the bucket bucketName.
try
{
    string bucketName = "mybucket"
    List<String> objectNames = new LinkedList<String>();
    objectNames.add("my-objectname1");
    objectNames.add("my-objectname2");
    objectNames.add("my-objectname3");
    RemoveObjectsAsync rmArgs = new RemoveObjectsAsync()
                                    .WithBucket(bucketName)
                                    .WithObjects(objectNames);
    IObservable<DeleteError> observable = await minio.RemoveObjectAsync(rmArgs);
    IDisposable subscription = observable.Subscribe(
        deleteError => Console.WriteLine("Object: {0}", deleteError.Key),
        ex => Console.WriteLine("OnError: {0}", ex),
        () =>
        {
            Console.WriteLine("Removed objects from " + bucketName + "\n");
        });
}
catch (MinioException e)
{
    Console.WriteLine("Error: " + e);
}

// 2. Remove list of objects (only specific versions mentioned in Version ID list) from the bucket bucketName
try
{
    string bucketName = "mybucket";
    string objectName = "myobject1";
    List<string> versionIDs = [];
    versionIDs.Add("abcobject1version1dce");
    versionIDs.Add("abcobject1version2dce");
    versionIDs.Add("abcobject1version3dce");
    List<Tuple<string, string>> objectsVersions = [];
    objectsVersions.Add(new Tuple<string, List<string>>(objectName, versionIDs));
    objectsVersions.Add(new Tuple<string, string>("myobject2" "abcobject2version1dce"));
    objectsVersions.Add(new Tuple<string, string>("myobject2", "abcobject2version2dce"));
    objectsVersions.Add(new Tuple<string, string>("myobject2", "abcobject2version3dce"));
    RemoveObjectsAsync rmArgs = new RemoveObjectsAsync()
                                    .WithBucket(bucketName)
                                    .WithObjectsVersions(objectsVersions);
    IObservable<DeleteError> observable = await minio.RemoveObjectsAsync(rmArgs);
    IDisposable subscription = observable.Subscribe(
        deleteError => Console.WriteLine("Object: {0}", deleteError.Key),
        ex => Console.WriteLine("OnError: {0}", ex),
        () =>
        {
            Console.WriteLine("Listed all delete errors for remove objects on  " + bucketName + "\n");
        });
}
catch (MinioException e)
{
    Console.WriteLine("Error: " + e);
}
```

### RemoveIncompleteUploadAsync(RemoveIncompleteUploadArgs args)

`Task RemoveIncompleteUploadAsync(RemoveIncompleteUploadArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Removes a partially uploaded object.

Parameters

Param

Type

Description

`args`

RemoveIncompleteUploadArgs

RemoveIncompleteUploadArgs object encapsulating the bucket, object names

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`InternalClientException` : upon internal library error

Example

```
try
{
    // Removes partially uploaded objects from buckets.
    RemoveIncompleteUploadArgs args = new RemoveIncompleteUploadArgs()
                                          .WithBucket(bucketName)
                                          .WithObject(objectName);
    await minioClient.RemoveIncompleteUploadAsync(args);
    Console.WriteLine("successfully removed all incomplete upload session of my-bucketname/my-objectname");
}
catch(MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### SelectObjectContentAsync(SelectObjectContentArgs args)

`Task<SelectResponseStream> SelectObjectContentAsync(SelectObjectContentArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Downloads an object as a stream.

Parameters

Param

Type

Description

`args`

SelectObjectContentArgs

options for SelectObjectContent async

Required parameter.

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`: Task callback returns a SelectResponseStream containing select results.

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name.

`ConnectionException` : upon connection error.

`InternalClientException` : upon internal library error.

`ArgumentException` : upon invalid response format

`IOException` : insufficient data

Example

```
try
{
    var opts = new SelectObjectOptions()
    {
        ExpressionType = QueryExpressionType.SQL,
        Expression = "select count(*) from s3object",
        InputSerialization = new SelectObjectInputSerialization()
        {
            CompressionType = SelectCompressionType.NONE,
            CSV = new CSVInputOptions()
            {
                FileHeaderInfo = CSVFileHeaderInfo.None,
                RecordDelimiter = "\n",
                FieldDelimiter = ",",
            }
        },
        OutputSerialization = new SelectObjectOutputSerialization()
        {
            CSV = new CSVOutputOptions()
            {
                RecordDelimiter = "\n",
                FieldDelimiter =  ",",
            }
        }
    };

    SelectObjectContentArgs args = SelectObjectContentArgs()
                                       .WithBucket(bucketName)
                                       .WithObject(objectName)
                                       .WithSelectObjectOptions(opts);
    var resp = await  minio.SelectObjectContentAsync(args);
    resp.Payload.CopyTo(Console.OpenStandardOutput());
    Console.WriteLine("Bytes scanned:" + resp.Stats.BytesScanned);
    Console.WriteLine("Bytes returned:" + resp.Stats.BytesReturned);
    Console.WriteLine("Bytes processed:" + resp.Stats.BytesProcessed);
    if (resp.Progress is not null)
    {
        Console.WriteLine("Progress :" + resp.Progress.BytesProcessed);
    }
}
catch (MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### SetObjectLegalHoldAsync(SetObjectLegalHoldArgs args)

`Task SetObjectLegalHoldAsync(SetObjectLegalHoldArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Sets the Legal Hold status of an object.

Parameters

Param

Type

Description

`args`

SetObjectLegalHoldArgs

SetObjectLegalHoldArgs Argument Object with bucket, object names, version id(optional)

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectNameException` : upon invalid object name

`BucketNotFoundException` : upon bucket with name not found

`ObjectNotFoundException` : upon object with name not found

`MissingObjectLockConfigurationException` : upon bucket created with object lock not enabled

`MalFormedXMLException` : upon configuration XML in http request validation failure

Example

```
try
{
    // Setting WithLegalHold true, sets Legal hold status to ON.
    SetObjectLegalHoldArgs args = new SetObjectLegalHoldArgs()
                                      .WithBucket(bucketName)
                                      .WithObject(objectName)
                                      .WithVersionId(versionId)
                                      .WithLegalHold(true);
    await minio.SetObjectLegalHoldAsync(args);
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### GetObjectLegalHoldAsync(GetObjectLegalHoldArgs args)

`Task<bool> GetObjectLegalHoldAsync(GetObjectLegalHoldArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Gets the Legal Hold status of an object.

Parameters

Param

Type

Description

`args`

GetObjectLegalHoldArgs

GetObjectLegalHoldArgs Argument Object with bucket, object names, version id(optional)

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<bool>`: True if LegalHold is enabled, false otherwise.

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectNameException` : upon invalid object name

`BucketNotFoundException` : upon bucket with name not found

`ObjectNotFoundException` : upon object with name not found

`MissingObjectLockConfigurationException` : upon bucket created with object lock not enabled

`MalFormedXMLException` : upon configuration XML in http request validation failure

Example

```
try
{
    // Get Legal Hold status a object
    var args = new GetObjectLegalHoldArgs()
                   .WithBucket(bucketName)
                   .WithObject(objectName)
                   .WithVersionId(versionId);
    bool enabled = await minio.GetObjectLegalHoldAsync(args);
    Console.WriteLine("LegalHold Configuration STATUS for " + bucketName + "/" + objectName +
                                        (!string.IsNullOrEmpty(versionId)?" with Version ID " + versionId: " ") +
                                        " : " + (enabled?"ON":"OFF"));
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### SetObjectTagsAsync(SetObjectTagsArgs args)

`Task SetObjectTagsAsync(SetObjectTagsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Sets tags to a object.

Parameters

Param

Type

Description

`args`

SetObjectTagsArgs

SetObjectTagsArgs Argument Object with object, tags to set

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectNameException` : upon invalid object name

`BucketNotFoundException` : upon bucket with name not found

`ObjectNotFoundException` : upon object with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Set Tags for the object
    SetObjectTagsArgs args = new SetObjectTagsArgs()
                                 .WithBucket(bucketName)
                                 .WithObject(objectName)
                                 .WithTagging(tags);
    await minio.SetObjectTagsAsync(args);
    Console.WriteLine($"Set tags for object {bucketName}/{objectName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### GetObjectTagsAsync(GetObjectTagsArgs args)

`Task<Tagging> GetObjectTagsAsync(GetObjectTagsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Gets tags of a object.

Parameters

Param

Type

Description

`args`

GetObjectTagsArgs

GetObjectTagsArgs Argument Object with object name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<Tagging>`: Task object which containing tag-value pairs.

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectNameException` : upon invalid object name

`BucketNotFoundException` : upon bucket with name not found

`ObjectNotFoundException` : upon object with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Get Object Tags for the object
    var args = new GetObjectTagsArgs()
                   .WithBucket(bucketName)
                   .WithObject(objectName);
    var tags = await minio.GetObjectTagsAsync(args);
    Console.WriteLine($"Got tags for object {bucketName}/{objectName}.");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### RemoveObjectTagsAsync(RemoveObjectTagsArgs args)

`Task RemoveObjectTagsAsync(RemoveObjectTagsArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Deletes tags of a object.

Parameters

Param

Type

Description

`args`

RemoveObjectTagsArgs

RemoveObjectTagsArgs Argument Object with object name

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectNameException` : upon invalid object name

`BucketNotFoundException` : upon bucket with name not found

`ObjectNotFoundException` : upon object with name not found

`MalFormedXMLException` : upon configuration XML in http request validation failure

`UnexpectedMinioException` : upon internal errors encountered during the operation

Example

```
try
{
    // Remove Tags for the object
    var args = new RemoveObjectTagsArgs()
                   .WithBucket(bucketName)
                   .WithObject(objectName);
    await minio.RemoveObjectTagsAsync(args);
    Console.WriteLine($"Removed tags for object {bucketName}/{objectName}.");
}
catch(MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### SetObjectRetentionAsync(SetObjectRetentionArgs args)

`Task SetObjectRetentionAsync(SetObjectRetentionArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Sets retention configuration to an object.

Parameters

Param

Type

Description

`args`

SetObjectRetentionArgs

SetObjectRetentionArgs Argument Object with bucket, object names, version id(optional)

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectNameException` : upon invalid object name

`BucketNotFoundException` : upon bucket with name not found

`ObjectNotFoundException` : upon object with name not found

`MissingObjectLockConfigurationException` : upon bucket created with object lock not enabled

`MalFormedXMLException` : upon configuration XML in http request validation failure

Example

```
try
{
    // Setting Retention Configuration of the object.
    SetObjectRetentionArgs args = new SetObjectRetentionArgs()
                                      .WithBucket(bucketName)
                                      .WithObject(objectName)
                                      .WithRetentionValidDays(numOfDays);
    await minio.SetObjectRetentionAsync(args);
    Console.WriteLine($"Assigned retention configuration to object {bucketName}/{objectName}");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### GetObjectRetentionAsync(GetObjectRetentionArgs args)

`Task<ObjectRetentionConfiguration> GetObjectRetentionAsync(GetObjectRetentionArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Gets retention configuration of an object.

Parameters

Param

Type

Description

`args`

GetObjectRetentionArgs

GetObjectRetentionArgs Argument Object with bucket, object names, version id(optional)

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task<ObjectRetentionConfiguration>`: ObjectRetentionConfiguration object with configuration data.

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectNameException` : upon invalid object name

`BucketNotFoundException` : upon bucket with name not found

`ObjectNotFoundException` : upon object with name not found

`MissingObjectLockConfigurationException` : upon bucket created with object lock not enabled

`MalFormedXMLException` : upon configuration XML in http request validation failure

Example

```
try
{
    // Get Retention configuration of an object
    var args = new GetObjectRetentionArgs()
                   .WithBucket(bucketName)
                   .WithObject(objectName);
    ObjectRetentionConfiguration config = await minio.GetObjectRetentionAsync(args);
    Console.WriteLine($"Got retention configuration for object {bucketName}/{objectName}");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

### ClearObjectRetentionAsync(ClearObjectRetentionArgs args)

`Task ClearObjectRetentionAsync(ClearObjectRetentionArgs args, CancellationToken cancellationToken = default(CancellationToken))`

Clears retention configuration to an object.

Parameters

Param

Type

Description

`args`

ClearObjectRetentionArgs

ClearObjectRetentionArgs Argument Object with bucket, object names, version id(optional)

`cancellationToken`

System.Threading.CancellationToken

Optional parameter. Defaults to default(CancellationToken)

Return Type

Exceptions

`Task`

Listed Exceptions:

`AuthorizationException` : upon access or secret key wrong or not found

`InvalidBucketNameException` : upon invalid bucket name

`InvalidObjectNameException` : upon invalid object name

`BucketNotFoundException` : upon bucket with name not found

`ObjectNotFoundException` : upon object with name not found

`MissingObjectLockConfigurationException` : upon bucket created with object lock not enabled

`MalFormedXMLException` : upon configuration XML in http request validation failure

Example

```
try
{
    // Clearing the Retention Configuration of the object.
    ClearObjectRetentionArgs args = new ClearObjectRetentionArgs()
                                        .WithBucket(bucketName)
                                        .WithObject(objectName);
    await minio.ClearObjectRetentionAsync(args);
    Console.WriteLine($"Clears retention configuration to object {bucketName}/{objectName}");
}
catch(MinioException e)
{
   Console.WriteLine("Error occurred: " + e);
}
```

## 4. Presigned operations

### PresignedGetObjectAsync(PresignedGetObjectArgs args);

`Task<string> PresignedGetObjectAsync(PresignedGetObjectArgs args)`

Generates a presigned URL for HTTP GET operations. Browsers/Mobile clients may point to this URL to directly download objects even if the bucket is private. This presigned URL can have an associated expiration time in seconds after which it is no longer operational. The default expiry is set to 7 days.

Parameters

Param

Type

Description

`args`

PresignedGetObjectArgs

PresignedGetObjectArgs encapsulating bucket, object names, expiry, response headers & request date

Return Type

Exceptions

`Task<string>` : string contains URL to download the object

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`InvalidExpiryRangeException` : upon invalid expiry range.

Example

```
try
{
    PresignedGetObjectArgs args = new PresignedGetObjectArgs()
                                      .WithBucket("mybucket")
                                      .WithObject("myobject")
                                      .WithExpiry(60 * 60 * 24);
    String url = await minioClient.PresignedGetObjectAsync(args);
    Console.WriteLine(url);
}
catch(MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### PresignedPutObjectAsync(PresignedPutObjectArgs args)

`Task<string> PresignedPutObjectAsync(PresignedPutObjectArgs args)`

Generates a presigned URL for HTTP PUT operations. Browsers/Mobile clients may point to this URL to upload objects directly to a bucket even if it is private. This presigned URL can have an associated expiration time in seconds after which it is no longer operational. The default expiry is set to 7 days.

Parameters

Param

Type

Description

`args`

PresignedPutObjectArgs

PresignedPutObjectArgs arguments object with bucket, object names & expiry

Return Type

Exceptions

`Task<string>` : string contains URL to upload the object

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`InvalidKeyException` : upon an invalid access key or secret key

`ConnectionException` : upon connection error

`InvalidExpiryRangeException` : upon invalid expiry range.

Example

```
try
{
    PresignedPutObjectArgs args = PresignedPutObjectArgs()
                                      .WithBucket("mybucket")
                                      .WithObject("myobject")
                                      .WithExpiry(60 * 60 * 24);
    String url = await minioClient.PresignedPutObjectAsync(args);
    Console.WriteLine(url);
}
catch(MinioException e)
{
    Console.WriteLine("Error occurred: " + e);
}
```

### PresignedPostPolicy(PresignedPostPolicyArgs args)

`Task<Dictionary<string, string>> PresignedPostPolicyAsync(PresignedPostPolicyArgs args)`

Allows setting policy conditions to a presigned URL for POST operations. Policies such as bucket name to receive object uploads, key name prefixes, expiry policy may be set.

Parameters

Param

Type

Description

`args`

PresignedPostPolicyArgs

PresignedPostPolicyArgs Arguments object includes bucket, object names & Post policy of an object.

Return Type

Exceptions

`Task<Dictionary<string, string>>`: Map of strings to construct form-data.

Listed Exceptions:

`InvalidBucketNameException` : upon invalid bucket name

`ConnectionException` : upon connection error

`NoSuchAlgorithmException` : upon requested algorithm was not found during signature calculation.

Example

```
try
{
    PostPolicy policy = new PostPolicy();
    policy.SetContentType("image/png");
    policy.SetUserMetadata("custom", "user");
    DateTime expiration = DateTime.UtcNow;
    policy.SetExpires(expiration.AddDays(10));
    policy.SetKey("my-objectname");
    policy.SetBucket("my-bucketname");
    PresignedPostPolicyArgs args = PresignedPostPolicyArgs()
                                       .WithBucket("my-bucketname")
                                       .WithObject("my-objectname")
                                       .WithPolicy(policy);

    Dictionary<string, string> formData = minioClient.Api.PresignedPostPolicy(args);
    string curlCommand = "curl ";
    foreach (KeyValuePair<string, string> pair in formData)
    {
        curlCommand = curlCommand + " -F " + pair.Key + "=" + pair.Value;
    }
    curlCommand = curlCommand + " -F file=@/etc/bashrc https://s3.amazonaws.com/my-bucketname";
    Console.WriteLine(curlCommand);
}
catch(MinioException e)
{
  Console.WriteLine("Error occurred: " + e);
}
```

## Client Custom Settings

### SetAppInfo(string appName, string appVersion)

Adds application details to User-Agent.

Parameters

Param

Type

Description

`appName`

string

Name of the application performing the API requests

`appVersion`

string

Version of the application performing the API requests

Example

```
// Set Application name and version to be used in subsequent API requests.
minioClient.SetAppInfo("myCloudApp", "1.0.0")
```

### SetTraceOn(IRequestLogger logger = null)

Enables HTTP tracing. The trace is written to the stdout.

Parameters

Param

Type

Description

`logger`

IRequestLogger

Implementation of interface `Minio.IRequestLogger` for serialization models for trace HTTP

Example

```
// Set HTTP tracing on with default trace logger.
minioClient.SetTraceOn()

// Set custom logger for HTTP trace
minioClient.SetTraceOn(new JsonNetLogger())
```

### SetTraceOff()

Disables HTTP tracing.

Example

```
// Sets HTTP tracing off.
minioClient.SetTraceOff()
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).