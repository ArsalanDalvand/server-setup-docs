---
title: "JavaScript Client API Reference — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/developers/javascript/API.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "developers/javascript/API"
category: "developers"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "JavaScript Client API Reference — SILO (MinIO Object Storage)"
---
# JavaScript Client API Reference

## Initialize MinIO Client object.

## MinIO

```
import * as Minio from 'minio'

const minioClient = new Minio.Client({
  endPoint: 'play.min.io',
  port: 9000,
  useSSL: true,
  accessKey: 'Q3AM3UQ867SPQQA43P2F',
  secretKey: 'zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG',
})
```

## AWS S3

```
import * as Minio from 'minio'

const s3Client = new Minio.Client({
  endPoint: 's3.amazonaws.com',
  accessKey: 'YOUR-ACCESSKEYID',
  secretKey: 'YOUR-SECRETACCESSKEY',
})
```

Bucket operations

Object operations

Presigned operations

Bucket Policy & Notification operations

Custom Settings

[`makeBucket`](#makeBucket)

[`getObject`](#getObject)

[`presignedUrl`](#presignedUrl)

[`getBucketNotification`](#getBucketNotification)

[`setS3TransferAccelerate`](#setS3TransferAccelerate)

[`listBuckets`](#listBuckets)

[`getPartialObject`](#getPartialObject)

[`presignedGetObject`](#presignedGetObject)

[`setBucketNotification`](#setBucketNotification)

[`bucketExists`](#bucketExists)

[`fGetObject`](#fGetObject)

[`presignedPutObject`](#presignedPutObject)

[`removeAllBucketNotification`](#removeAllBucketNotification)

[`removeBucket`](#removeBucket)

[`putObject`](#putObject)

[`presignedPostPolicy`](#presignedPostPolicy)

[`getBucketPolicy`](#getBucketPolicy)

[`listObjects`](#listObjects)

[`fPutObject`](#fPutObject)

[`setBucketPolicy`](#setBucketPolicy)

[`listObjectsV2`](#listObjectsV2)

[`copyObject`](#copyObject)

[`listenBucketNotification`](#listenBucketNotification)

[`listIncompleteUploads`](#listIncompleteUploads)

[`statObject`](#statObject)

[`getBucketVersioning`](#getBucketVersioning)

[`removeObject`](#removeObject)

[`setBucketVersioning`](#setBucketVersioning)

[`removeObjects`](#removeObjects)

[`getBucketTagging`](#getBucketTagging)

[`removeIncompleteUpload`](#removeIncompleteUpload)

[`setBucketTagging`](#setBucketTagging)

[`putObjectRetention`](#putObjectRetention)

[`removeBucketTagging`](#removeBucketTagging)

[`getObjectRetention`](#getObjectRetention)

[`setBucketLifecycle`](#setBucketLifecycle)

[`setObjectTagging`](#setObjectTagging)

[`getBucketLifecycle`](#getBucketLifecycle)

[`removeObjectTagging`](#removeObjectTagging)

[`removeBucketLifecycle`](#removeBucketLifecycle)

[`getObjectTagging`](#getObjectTagging)

[`setObjectLockConfig`](#setObjectLockConfig)

[`getObjectLegalHold`](#getObjectLegalHold)

[`getObjectLockConfig`](#getObjectLockConfig)

[`setObjectLegalHold`](#setObjectLegalHold)

[`getBucketEncryption`](#getBucketEncryption)

[`composeObject`](#composeObject)

[`setBucketEncryption`](#setBucketEncryption)

[`selectObjectContent`](#selectObjectContent)

[`removeBucketEncryption`](#removeBucketEncryption)

[`setBucketReplication`](#setBucketReplication)

[`getBucketReplication`](#getBucketReplication)

[`removeBucketReplication`](#removeBucketReplication)

## 1. Constructor

### new Minio.Client ({endPoint, port, useSSL, accessKey, secretKey, region, transport, sessionToken, partSize})

`new Minio.Client ({endPoint, port, useSSL, accessKey, secretKey, region, transport, sessionToken, partSize})`

Initializes a new client object.

Parameters

Param

Type

Description

`endPoint`

string

endPoint is a host name or an IP address.

`port`

number

TCP/IP port number. This input is optional. Default value set to 80 for HTTP and 443 for HTTPs.

`useSSL`

bool

If set to true, https is used instead of http. Default is true.

`accessKey`

string

accessKey is like user-id that uniquely identifies your account.

`secretKey`

string

secretKey is the password to your account.

`sessionToken`

string

Set this value to provide x-amz-security-token (AWS S3 specific). (Optional)

`region`

string

Set this value to override region cache. (Optional)

`transport`

string

Set this value to pass in a custom transport. (Optional)

`partSize`

number

Set this value to override default part size of 64MB for multipart uploads. (Optional)

`pathStyle`

bool

Set this value to override default access behavior (path) for non AWS endpoints. Default is true. (Optional)

`transportAgent`

[Agent](https://nodejs.org/api/http.html#class-httpagent)

Set this value to provide a custom HTTP(s) agent to handle timeouts, TLS handling, and low-level socket configurations. (Optional)

Example

## Create client for MinIO

```
import * as Minio from 'minio'

const minioClient = new Minio.Client({
  endPoint: 'play.min.io',
  port: 9000,
  useSSL: true,
  accessKey: 'Q3AM3UQ867SPQQA43P2F',
  secretKey: 'zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG',
})
```

## Create client for AWS S3

```
import * as Minio from 'minio'

const s3Client = new Minio.Client({
  endPoint: 's3.amazonaws.com',
  accessKey: 'YOUR-ACCESSKEYID',
  secretKey: 'YOUR-SECRETACCESSKEY',
})
```

## Create client with temporary credentials

```
import * as Minio from 'minio'

const s3Client = new Minio.Client({
  endPoint: 's3.amazonaws.com',
  accessKey: 'YOUR-TEMP-ACCESSKEYID',
  secretKey: 'YOUR-TEMP-SECRETACCESSKEY',
  sessionToken: 'YOUR-TEMP-SESSIONTOKEN',
})
```

## Create client with custom HTTPS Agent

```
import * as Minio from 'minio'
import * as fs from 'fs'
import * as https from 'https'

const s3Client = new Minio.Client({
  endPoint: 'play.min.io',
  port: 9000,
  useSSL: true,
  accessKey: 'Q3AM3UQ867SPQQA43P2F',
  secretKey: 'zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG',
  transportAgent: new https.Agent({
    timeout: 10000,
    ca: fs.readFileSync('path/to/ca.cert'),
    cert: fs.readFileSync('path/to/public.cert'),
    key: fs.readFileSync('path/to/secret.key'),
    keepAlive: false,
  }),
})
```

### Note: The below examples may rely on top level await.

## 2. Bucket operations

### async makeBucket(bucketName, [region, makeOpts]): Promise

Creates a new bucket.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`region`

string

Region where the bucket is created. This parameter is optional. Default value is us-east-1.

`makeOpts`

object

Options to create a bucket. e.g `{ObjectLocking:true}` (Optional)

Example

```
await minioClient.makeBucket('mybucket', 'us-east-1')
console.log('Bucket created successfully in "us-east-1".')
```

Example 1
Create a bucket with object locking enabled.

```
minioClient.makeBucket('mybucket', 'us-east-1', { ObjectLocking: true }, function (err) {
  if (err) return console.log('Error creating bucket with object lock.', err)
  console.log('Bucket created successfully in "us-east-1" and enabled object lock')
})
```

### listBuckets()

Lists all buckets.

Parameters

NIL

Returns Array of Objects with the format:-

Param

Type

Description

`bucket.name`

string

bucket name

`bucket.creationDate`

Date

date when bucket was created.

Example

Please refer to: [list-buckets.mjs](#..%2Fexamples%2Flist-buckets.mjs)

```
try {
  const buckets = await minioClient.listBuckets()
  console.log('Success', buckets)
} catch (err) {
  console.log(err.message)
}
```

#### async bucketExists(bucketName): Promise

Checks if a bucket exists.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

Example

```
const exists = await minioClient.bucketExists('mybucket')
if (exists) {
  return console.log('Bucket exists.')
}
```

### removeBucket(bucketName[, callback])

Removes a bucket.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`callback(err)`

function

`err` is `null` if the bucket is removed successfully. If no callback is passed, a `Promise` is returned.

Example

```
try {
  await minioClient.removeBucket('mybucket')
  console.log('Bucket removed successfully.')
} catch (err) {
  console.log('unable to remove bucket.')
}
```

### listObjects(bucketName, prefix, recursive [,listOpts])

Lists all objects in a bucket.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`prefix`

string

The prefix of the objects that should be listed (optional, default `''`).

`recursive`

bool

`true` indicates recursive style listing and `false` indicates directory style listing delimited by ‘/’. (optional, default `false`).

`listOpts`

object

query params to list object which can have `{IncludeVersion: _bool_ }` (optional)

Return Value

Param

Type

Description

`stream`

Stream

Stream emitting the objects in the bucket.

The object is of the format:

Param

Type

Description

`obj.name`

string

name of the object.

`obj.prefix`

string

name of the object prefix.

`obj.size`

number

size of the object.

`obj.etag`

string

etag of the object.

`obj.versionId`

string

versionId of the object.

`obj.isDeleteMarker`

boolean

true if it is a delete marker.

`obj.lastModified`

Date

modified time stamp.

Example

```
const data = []
const stream = minioClient.listObjects('mybucket', '', true)
stream.on('data', function (obj) {
  data.push(obj)
})
stream.on('end', function () {
  console.log(data)
})
stream.on('error', function (err) {
  console.log(err)
})
```

Example1
To get Object versions

```
const data = []
const stream = minioClient.listObjects('mybucket', '', true, { IncludeVersion: true })
stream.on('data', function (obj) {
  data.push(obj)
})
stream.on('end', function () {
  console.log(data)
})
stream.on('error', function (err) {
  console.log(err)
})
```

### listObjectsV2(bucketName, prefix, recursive, startAfter)

Lists all objects in a bucket using S3 listing objects V2 API

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`prefix`

string

The prefix of the objects that should be listed (optional, default `''`).

`recursive`

bool

`true` indicates recursive style listing and `false` indicates directory style listing delimited by ‘/’. (optional, default `false`).

`startAfter`

string

Specifies the object name to start after when listing objects in a bucket. (optional, default `''`).

Return Value

Param

Type

Description

`stream`

Stream

Stream emitting the objects in the bucket.

The object is of the format:

Param

Type

Description

`obj.name`

string

name of the object.

`obj.prefix`

string

name of the object prefix.

`obj.size`

number

size of the object.

`obj.etag`

string

etag of the object.

`obj.lastModified`

Date

modified time stamp.

Example

```
const stream = minioClient.listObjectsV2('mybucket', '', true, '')
stream.on('data', function (obj) {
  console.log(obj)
})
stream.on('error', function (err) {
  console.log(err)
})
```

### listObjectsV2WithMetadata(bucketName, prefix, recursive, startAfter)

Lists all objects and their metadata in a bucket using S3 listing objects V2 API

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`prefix`

string

The prefix of the objects that should be listed (optional, default `''`).

`recursive`

bool

`true` indicates recursive style listing and `false` indicates directory style listing delimited by ‘/’. (optional, default `false`).

`startAfter`

string

Specifies the object name to start after when listing objects in a bucket. (optional, default `''`).

Return Value

Param

Type

Description

`stream`

Stream

Stream emitting the objects in the bucket.

The object is of the format:

Param

Type

Description

`obj.name`

string

name of the object.

`obj.prefix`

string

name of the object prefix.

`obj.size`

number

size of the object.

`obj.etag`

string

etag of the object.

`obj.lastModified`

Date

modified time stamp.

`obj.metadata`

object

metadata of the object.

Example

```
const stream = minioClient.extensions.listObjectsV2WithMetadata('mybucket', '', true, '')
stream.on('data', function (obj) {
  console.log(obj)
})
stream.on('error', function (err) {
  console.log(err)
})
```

### listIncompleteUploads(bucketName, prefix, recursive)

Lists partially uploaded objects in a bucket.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`prefix`

string

Prefix of the object names that are partially uploaded. (optional, default `''`)

`recursive`

bool

`true` indicates recursive style listing and `false` indicates directory style listing delimited by ‘/’. (optional, default `false`).

Return Value

Param

Type

Description

`stream`

Stream

Emits objects of the format listed below:

Param

Type

Description

`part.key`

string

name of the object.

`part.uploadId`

string

upload ID of the object.

`part.size`

Integer

size of the partially uploaded object.

Example

```
const Stream = minioClient.listIncompleteUploads('mybucket', '', true)
Stream.on('data', function (obj) {
  console.log(obj)
})
Stream.on('end', function () {
  console.log('End')
})
Stream.on('error', function (err) {
  console.log(err)
})
```

### getBucketVersioning(bucketName)

Get Versioning state of a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

Example

```
const versionInfo = await minioClient.getBucketVersioning('bucketname')
console.log('Success ', versionInfo)
```

### setBucketVersioning(bucketName, versioningConfig)

Set Versioning state on a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`versioningConfig`

object

Versioning Configuration e.g: `{Status:"Enabled"}`

Example

```
const versioningConfig = { Status: 'Enabled' }
await minioClient.setBucketVersioning('bucketname', versioningConfig)
```

### setBucketReplication(bucketName, replicationConfig)

Set replication config on a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`replicationConfig`

object

replicationConfig Configuration as a JSON Object

Example

```
const arnFromMcCli = 'arn:minio:replication::b22d653b-e4fb-4c5d-8140-7694c8e72ed4:dest-bucket'
const replicationConfig = {
  role: arnFromMcCli,
  rules: [
    {
      ID: 'cisea130mbms6splbmg0',
      Status: 'Enabled',
      Priority: 1,
      DeleteMarkerReplication: { Status: 'Enabled' },
      DeleteReplication: { Status: 'Enabled' },
      Destination: {
        Bucket: 'arn:aws:s3:::dest-bucket',
        StorageClass: 'REDUCED_REDUNDANCY',
      },
      SourceSelectionCriteria: { ReplicaModifications: { Status: 'Enabled' } },
      Filter: {
        //Possible values.
        // Prefix: '/',
        // Tag: [{ 'Key': 'key1', 'Value': 'value1' }, { 'Key': 'key2', 'Value': 'value2' }],//if only this,  =>    'DeleteMarkerReplication': { 'Status': 'Disabled' },
        And: {
          Prefix: '/',
          Tag: [
            { Key: 'key1', Value: 'value1' },
            { Key: 'key2', Value: 'value2' },
          ],
        },
      },
      ExistingObjectReplication: { Status: 'Enabled' },
    },
  ],
}

await s3Client.setBucketReplication('source-bucket', replicationConfig)
```

### getBucketReplication(bucketName)

Get replication config of a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

Example

```
const replicatinConfig = await minioClient.getBucketReplication('source-bucket')
console.log(replicatinConfig)
```

### removeBucketReplication(bucketName)

Remove replication config of a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

Example

```
await minioClient.removeBucketReplication('source-bucket')
```

### setBucketTagging(bucketName, tags)

Set Tags on a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`tags`

object

Tags map Configuration e.g: `{<tag-key-1>:<tag-value-1>}`

Example

```
await minioClient.setBucketTagging('bucketname', tags)
```

### removeBucketTagging(bucketName, callback)

Remove Tags on a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

Example

```
await minioClient.removeBucketTagging('bucketname')
```

### getBucketTagging(bucketName)

Gets Tags on a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

Example

```
const tagList = await minioClient.getBucketTagging('bucketname')
console.log(tagList)
```

### setBucketLifecycle(bucketName, lifecycleConfig)

Set Lifecycle Configuration on a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`lifecycleConfig`

object

Valid Lifecycle Configuration or ( `null` or `''` ) to remove policy configuration

Example

```
const lifecycleConfig = {
  Rule: [
    {
      ID: 'Transition and Expiration Rule',
      Status: 'Enabled',
      Filter: {
        Prefix: '',
      },
      Expiration: {
        Days: '3650',
      },
    },
  ],
}

await minioClient.setBucketLifecycle('bucketname', lifecycleConfig)
```

### getBucketLifecycle(bucketName)

Get Lifecycle Configuration of a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

Example

```
await minioClient.getBucketLifecycle('bucketname')
```

### removeBucketLifecycle(bucketName)

Remove Lifecycle Configuration of a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

Example

```
await minioClient.removeBucketLifecycle('bucketname')
```

### setObjectLockConfig(bucketName, lockConfig [, callback])

Set Object lock config on a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`lockConfig`

object

Lock Configuration can be either `{}` to reset or object with all of the following key/value pairs: `{mode: ["COMPLIANCE"/'GOVERNANCE'], unit: ["Days"/"Years"], validity: <a-valid-number-for-unit>}`

Example 1

```
await minioClient.setObjectLockConfig('my-bucketname', { mode: 'COMPLIANCE', unit: 'Days', validity: 10 })
```

Example 2
To reset/remove object lock config on a bucket.

```
await s3Client.setObjectLockConfig('my-bucketname', {})
```

### getObjectLockConfig(bucketName [, callback])

Get Lock config on a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

**Example **
Get object lock configuration on a Bucket

```
await minioClient.getObjectLockConfig('my-bucketname')
```

### setBucketEncryption(bucketName [,encryptionConfig])

Set encryption configuration on a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`encryptionConfig`

object

Encryption Configuration can be either omitted or `{}` or a valid and supported encryption config. by default: `{Rule:[{ApplyServerSideEncryptionByDefault:{SSEAlgorithm:"AES256"}}]}` is applied.

**Example **
Set Encryption configuration on a Bucket

```
await s3Client.setBucketEncryption('my-bucketname')
```

Example 1
Set Encryption configuration on a Bucket with an Algorithm

```
await s3Client.setBucketEncryption('my-bucketname', {
  Rule: [{ ApplyServerSideEncryptionByDefault: { SSEAlgorithm: 'AES256' } }],
})
```

### getBucketEncryption(bucketName)

Get encryption configuration of a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

**Example **
Get Encryption configuration of a Bucket

```
await s3Client.getBucketEncryption('my-bucketname')
```

### removeBucketEncryption(bucketName)

Remove encryption configuration of a Bucket

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

**Example **
Remove Encryption configuration of a Bucket

```
await s3Client.removeBucketEncryption('my-bucketname')
```

## 3. Object operations

### getObject(bucketName, objectName, getOpts)

Downloads an object as a stream.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`getOpts`

object

Options to get the object. Default is `{}`. (optional)

Return Value

Param

Type

Description

`stream`

`stream.Readable`

Stream emitting the object content.

Example

```
let size = 0
const dataStream = await minioClient.getObject('mybucket', 'photo.jpg')
dataStream.on('data', function (chunk) {
  size += chunk.length
})
dataStream.on('end', function () {
  console.log('End. Total size = ' + size)
})
dataStream.on('error', function (err) {
  console.log(err)
})
```

Example

Get a specific object version.

```
let size = 0
const dataStream = await minioClient.getObject('mybucket', 'photo.jpg', { versionId: 'my-versionId' })
dataStream.on('data', function (chunk) {
  size += chunk.length
})
dataStream.on('end', function () {
  console.log('End. Total size = ' + size)
})
dataStream.on('error', function (err) {
  console.log(err)
})
```

Example

Get a Server Side Encrypted object.

```
let size = 0
const dataStream = await minioClient.getObject('mybucket', 'photo.jpg', {
  SSECustomerAlgorithm: 'AES256',
  SSECustomerKey: 'YOUR_KEY',
  SSECustomerKeyMD5: 'YOUR_MD5',
})
dataStream.on('data', function (chunk) {
  size += chunk.length
})
dataStream.on('end', function () {
  console.log('End. Total size = ' + size)
})
dataStream.on('error', function (err) {
  console.log(err)
})
```

### getPartialObject(bucketName, objectName, offset, length, getOpts[, callback])

Downloads the specified range bytes of an object as a stream.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`offset`

number

`offset` of the object from where the stream will start.

`length`

number

`length` of the object that will be read in the stream (optional, if not specified we read the rest of the file from the offset).

`getOpts`

object

Options to get the object. Default is `{}`. (optional)

`callback(err, stream)`

function

Callback is called with `err` in case of error. `stream` is the object content stream. If no callback is passed, a `Promise` is returned.

Return Value

Param

Type

Description

`stream`

Stream

Stream emitting the object content.

Example

```
let size = 0
// reads 30 bytes from the offset 10.
const dataStream = await minioClient.getPartialObject('mybucket', 'photo.jpg', 10, 30)
dataStream.on('data', function (chunk) {
  size += chunk.length
})
dataStream.on('end', function () {
  console.log('End. Total size = ' + size)
})
dataStream.on('error', function (err) {
  console.log(err)
})
```

Example
To get a specific version of an object

```
const versionedObjSize = 0
// reads 30 bytes from the offset 10.
const dataStream = await minioClient.getPartialObject('mybucket', 'photo.jpg', 10, 30, { versionId: 'my-versionId' })
dataStream.on('data', function (chunk) {
  versionedObjSize += chunk.length
})
dataStream.on('end', function () {
  console.log('End. Total size = ' + versionedObjSize)
})
dataStream.on('error', function (err) {
  console.log(err)
})
```

Example
To get a Server Side Encrypted object.

```
const versionedObjSize = 0
// reads 30 bytes from the offset 10.
const dataStream = await minioClient.getPartialObject('mybucket', 'photo.jpg', 10, 30, {
  SSECustomerAlgorithm: 'AES256',
  SSECustomerKey: 'YOUR_KEY',
  SSECustomerKeyMD5: 'YOUR_MD5',
})
dataStream.on('data', function (chunk) {
  versionedObjSize += chunk.length
})
dataStream.on('end', function () {
  console.log('End. Total size = ' + versionedObjSize)
})
dataStream.on('error', function (err) {
  console.log(err)
})
```

### fGetObject(bucketName, objectName, filePath, getOpts[, callback])

Downloads and saves the object as a file in the local filesystem.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`filePath`

string

Path on the local filesystem to which the object data will be written.

`getOpts`

object

Options to get the object. Default is `{}`. (optional)

`callback(err)`

function

Callback is called with `err` in case of error. If no callback is passed, a `Promise` is returned.

Return Value

Value

Type

Description

`err`

object

Error in case of any failures

`file`

file

Streamed Output file at the specified `filePath`

Example

```
minioClient.fGetObject('mybucket', 'photo.jpg', '/tmp/photo.jpg', function (err) {
  if (err) {
    return console.log(err)
  }
  console.log('success')
})
```

Example
To Stream a specific object version into a file.

```
minioClient.fGetObject(bucketName, objNameValue, './download/MyImage.jpg', { versionId: 'my-versionId' }, function (e) {
  if (e) {
    return console.log(e)
  }
  console.log('success')
})
```

Example
To Stream a Server Side Encrypted object into a file.

```
minioClient.fGetObject(
  bucketName,
  objNameValue,
  './download/MyImage.jpg',
  {
    SSECustomerAlgorithm: 'AES256',
    SSECustomerKey: 'YOUR_KEY',
    SSECustomerKeyMD5: 'YOUR_MD5',
  },
  function (e) {
    if (e) {
      return console.log(e)
    }
    console.log('success')
  },
)
```

### putObject(bucketName, objectName, stream, size, metaData[, callback])

Uploads an object from a stream/Buffer.

#### From a stream

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`stream`

Stream

Readable stream.

`size`

number

Size of the object (optional).

`metaData`

Javascript Object

metaData of the object (optional).

`callback(err, objInfo)`

function

Non-null `err` indicates error, in case of Success,`objInfo` contains `etag` string and `versionId` string of the object. If no callback is passed, a `Promise` is returned.

Return Value

Value

Type

Description

`err`

object

Error in case of any failures

`objInfo.etag`

string

`etag` of an object

`objInfo.versionId`

string

`versionId` of an object (optional)

Example

The maximum size of a single object is limited to 5TB. putObject transparently uploads objects larger than 64MiB in multiple parts. Uploaded data is carefully verified using MD5SUM signatures.

```
import * as Fs from 'fs'
const file = '/tmp/40mbfile'
const fileStream = Fs.createReadStream(file)
const fileStat = Fs.stat(file, function (err, stats) {
  if (err) {
    return console.log(err)
  }
  minioClient.putObject('mybucket', '40mbfile', fileStream, stats.size, function (err, objInfo) {
    if (err) {
      return console.log(err) // err should be null
    }
    console.log('Success', objInfo)
  })
})
```

#### From a “Buffer” or a “string”

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`string or Buffer`

Stream or Buffer

Readable stream.

`metaData`

Javascript Object

metaData of the object (optional).

`callback(err, etag)`

function

Non-null `err` indicates error, `etag` string is the etag of the object uploaded.

Example

```
const buffer = 'Hello World'
minioClient.putObject('mybucket', 'hello-file', buffer, function (err, etag) {
  return console.log(err, etag) // err should be null
})
```

### fPutObject(bucketName, objectName, filePath, metaData[, callback])

Uploads contents from a file to objectName.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`filePath`

string

Path of the file to be uploaded.

`metaData`

Javascript Object

Metadata of the object.

`callback(err, objInfo)` function: non null `err` indicates error, `objInfo` object is the information about the object uploaded which contains `versionId` string and `etag` string.

Return Value

Value

Type

Description

`err`

object

Error in case of any failures

`objInfo.etag`

string

`etag` of an object

`objInfo.versionId`

string

`versionId` of an object (optional)

Example

The maximum size of a single object is limited to 5TB. fPutObject transparently uploads objects larger than 64MiB in multiple parts. Uploaded data is carefully verified using MD5SUM signatures.

```
const file = '/tmp/40mbfile'
const metaData = {
  'Content-Type': 'text/html',
  'Content-Language': 123,
  'X-Amz-Meta-Testing': 1234,
  example: 5678,
}
minioClient.fPutObject('mybucket', '40mbfile', file, metaData, function (err, objInfo) {
  if (err) {
    return console.log(err)
  }
  console.log('Success', objInfo.etag, objInfo.versionId)
})
```

### copyObject(targetBucketName, targetObjectName, sourceBucketNameAndObjectName [,conditions])

Copy a source object into a new object in the specified bucket.

Parameters

Param

Type

Description

`targetBucketName`

string

Name of the bucket.

`targetObjectName`

string

Name of the object.

`sourceBucketNameAndObjectName`

string

Path of the file to be copied.

`conditions`

CopyConditions

Conditions to be satisfied before allowing object copy.

Example

```
const conds = new Minio.CopyConditions()
conds.setMatchETag('bd891862ea3e22c93ed53a098218791d')
await minioClient.copyObject('mybucket', 'newobject', '/mybucket/srcobject', conds)
```

### statObject(bucketName, objectName, statOpts[, callback])

Gets metadata of an object.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`statOpts`

object

Version of the object in the form `{versionId:"my-versionId"}`. Default is `{}`. (optional)

`callback(err, stat)`

function

`err` is not `null` in case of error, `stat` contains the object information listed below. If no callback is passed, a `Promise` is returned.

Return Value

Param

Type

Description

`stat.size`

number

size of the object.

`stat.etag`

string

etag of the object.

`stat.versionId`

string

version of the object.

`stat.metaData`

Javascript Object

metadata of the object.

`stat.lastModified`

Date

Last Modified time stamp.

Example

```
const stat = await minioClient.statObject('mybucket', 'photo.jpg')
console.log(stat)
```

Example stat on a version of an object

```
const stat = await minioClient.statObject('mybucket', 'photo.jpg', { versionId: 'my-versionId' })
console.log(stat)
```

### removeObject(bucketName, objectName [, removeOpts])

Removes an object.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`removeOpts`

object

Version of the object in the form `{versionId:"my-versionId", governanceBypass: true or false }`. Default is `{}`. (Optional)

Example 1

```
;(async function () {
  await minioClient.removeObject('mybucket', 'photo.jpg')
  console.log('Removed the object')
})()
```

Example 2
Delete a specific version of an object

```
;(async function () {
  try {
    await minioClient.removeObject('mybucket', 'photo.jpg', { versionId: 'my-versionId' })
    console.log('Removed the object')
  } catch (err) {
    console.log('Unable to remove object', err)
  }
})()
```

Example 3
Remove an object version locked with retention mode `GOVERNANCE` using the `governanceBypass` remove option

```
;(async function () {
  await s3Client.removeObject('my-bucketname', 'my-objectname', { versionId: 'my-versionId', governanceBypass: true })
  console.log('Success')
})()
```

### removeObjects(bucketName, objectsList)

Remove all objects in the objectsList.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectsList`

object

list of objects in the bucket to be removed. any one of the formats: 1. List of Object names as array of strings which are object keys: `['objectname1','objectname2']` 2. List of Object name and VersionId as an object: [{name:”my-obj-name”,versionId:”my-versionId”}]

Example

```
const objectsList = []

// List all object paths in bucket my-bucketname.
const objectsStream = s3Client.listObjects('my-bucketname', 'my-prefixname', true)

objectsStream.on('data', function (obj) {
  objectsList.push(obj.name)
})

objectsStream.on('error', function (e) {
  console.log(e)
})

objectsStream.on('end', async () => {
  await s3Client.removeObjects(bucket, objectsList)
})
```

Example1

With versioning Support

```
const objectsList = []
const bucket = 'my-bucket'
const prefix = 'my-prefix'
const recursive = false

const objectsStream = s3Client.listObjects(bucket, prefix, recursive, { IncludeVersion: true })
objectsStream.on('data', function (obj) {
  objectsList.push(obj)
})
objectsStream.on('error', function (e) {
  return console.log(e)
})
objectsStream.on('end', async () => {
  await s3Client.removeObjects(bucket, objectsList)
})
```

### removeIncompleteUpload(bucketName, objectName)

Removes a partially uploaded object.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

Example

```
await minioClient.removeIncompleteUpload('mybucket', 'photo.jpg')
```

### async putObjectRetention(bucketName, objectName [, retentionOpts])

Apply retention on an object.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`retentionOpts`

object

Options for retention like : `{ governanceBypass:true/false ,mode:COMPLIANCE/GOVERNANCE, retainUntilDate: _date_ , versionId:"my-versionId" }` Default is `{}` (Optional)

Example
Apply object retention on an object

```
const bucketName = 'my-bucket'
const objectName = 'my-object'

const expirationDate = new Date()
expirationDate.setDate(expirationDate.getDate() + 1)
expirationDate.setUTCHours(0, 0, 0, 0) //Should be start of the day.(midnight)
const versionId = 'e67b4b08-144d-4fc4-ba15-43c3f7f9ba74'

await minioClient.putObjectRetention(bucketName, objectName, {
  Mode: 'GOVERNANCE',
  retainUntilDate: retainUntilDate.toISOString(),
  versionId: versionId,
})
```

### getObjectRetention(bucketName, objectName [, getOpts])

Get retention config of an object

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`getOpts`

object

Options for retention like : `{ versionId:"my-versionId" }` Default is `{}` (Optional)

Example 1

```
const retentionInfo = await minioClient.getObjectRetention('bucketname', 'objectname')
console.log(retentionInfo)
```

Example 2

```
const retInfoForVersionId = await minioClient.getObjectRetention('bucketname', 'objectname', {
  versionId: 'my-versionId',
})
console.log(retInfoForVersionId)
```

### setObjectTagging(bucketName, objectName, tags[, putOpts])

Put Tags on an Object

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`tags`

object

Tags map Configuration e.g: `{<tag-key-1>:<tag-value-1>}`

`putOpts`

object

Default is {}. e.g `{versionId:"my-version-id"}`. (Optional)

Example

```
await minioClient.setObjectTagging('bucketname', 'object-name', tags)
```

Example 1
Put tags on a version of an object.

```
await minioClient.setObjectTagging('bucketname', 'object-name', tags, { versionId: 'my-version-id' })
```

### removeObjectTagging(bucketName, objectName[, removeOpts])

Remove Tags on an Object

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`removeOpts`

object

Defaults to {}. e.g `{versionId:"my-version-id"}`. (Optional)

Example

```
await minioClient.removeObjectTagging('bucketname', 'object-name')
```

Example1
Remove tags on a version of an object.

```
await minioClient.removeObjectTagging('bucketname', 'object-name', { versionId: 'my-object-version-id' })
```

### getObjectTagging(bucketName, objectName[, getOpts])

Get Tags of an Object

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`getOpts`

object

Defaults to {}. e.g `{versionId:"my-version-id"}`. (Optional)

Example

```
console.log(await minioClient.getObjectTagging('bucketname', 'object-name'))
```

Example1
Get tags on a version of an object.

```
console.log(await minioClient.getObjectTagging('bucketname', 'object-name', { versionId: 'my-object-version-id' }))
```

### getObjectLegalHold(bucketName, objectName, getOpts)

Get legal hold on an object.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`getOpts`

object

Legal hold configuration options. e.g `{versionId:'my-version-uuid'}`. Defaults to `{}` .

Example 1

Get Legal hold of an object.

```
const legalholdStatus = await minioClient.getObjectLegalHold('bucketName', 'objectName')
```

Example 2

Get Legal hold of an object with versionId.

```
const legalholdStatus = await minioClient.getObjectLegalHold('bucketName', 'objectName', {
  versionId: 'my-obj-version-uuid',
})
```

### setObjectLegalHold(bucketName, objectName, [,setOpts])

Set legal hold on an object.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`setOpts`

object

Legal hold configuration options to set. e.g `{versionId:'my-version-uuid', status:'ON or OFF'}`. Defaults to `{status:'ON'}` if not passed.

Example 1

Set Legal hold of an object.

```
const legalholdStatus = await minioClient.setObjectLegalHold('bucketName', 'objectName', { Status: 'ON' })
```

Example 2

Set Legal hold of an object with versionId.

```
const legalholdStatus = await minioClient.setObjectLegalHold('bucketName', 'objectName', {
  Status: 'ON',
  versionId: 'my-obj-version-uuid',
})
```

### composeObject(destObjConfig, sourceObjectList)

Compose an object from parts

Parameters

Param

Type

Description

`destObjConfig`

object

Destination Object configuration of the type [CopyDestinationOptions](https://github.com/minio/minio-js/blob/master/src/helpers.js)

`sourceObjectList`

object[]

Array of object(parts) source to compose into an object. Each part configuration should be of type [CopySourceOptions](https://github.com/minio/minio-js/blob/master/src/helpers.js)

Example 1

Compose an Object from its parts .

```
import * as minio from 'minio'

const sourceList = [
  new minio.CopySourceOptions({
    Bucket: 'source-bucket',
    Object: 'parta',
  }),
  new minio.CopySourceOptions({
    Bucket: 'source-bucket',
    Object: 'partb',
  }),
  new minio.CopySourceOptions({
    Bucket: 'source-bucket',
    Object: 'partc',
  }),
  new minio.CopySourceOptions({
    Bucket: 'source-bucket',
    Object: 'partd',
  }),
]

const destOption = new minio.CopyDestinationOptions({
  Bucket: 'dest-bucket',
  Object: '100MB.zip',
})

//using Promise style.
await minioClient.composeObject(destOption, sourceList)
```

### selectObjectContent(bucketName, objectName, selectOpts)

Select contents of an object (S3 Select).

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`selectOpts`

object

Example 1
Select all values

```
const selectOpts = {
  expression: 'SELECT * FROM s3object s where s."Name" = \'Jane\'',
  expressionType: 'SQL',
  inputSerialization: {
    CSV: { FileHeaderInfo: 'Use', RecordDelimiter: '\n', FieldDelimiter: ',' },
    CompressionType: 'NONE',
  },
  outputSerialization: { CSV: { RecordDelimiter: '\n', FieldDelimiter: ',' } },
  requestProgress: { Enabled: true },
}

const res = await minioClient.selectObjectContent('bucketName', 'objectName', selectOpts)
console.log(res)
```

## 4. Presigned operations

Presigned URLs are generated for temporary download/upload access to private objects.

### presignedUrl(httpMethod, bucketName, objectName[, expiry, reqParams, requestDate])

Generates a presigned URL for the provided HTTP method, ‘httpMethod’. Browsers/Mobile clients may point to this URL to directly download objects even if the bucket is private. This presigned URL can have an associated expiration time in seconds after which the URL is no longer valid. The default value is 7 days.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`expiry`

number

Expiry time in seconds. Default value is 7 days. (optional)

`reqParams`

object

request parameters. (optional) e.g {versionId:”10fa9946-3f64-4137-a58f-888065c0732e”}

`requestDate`

Date

A date object, the url will be issued at. Default value is now. (optional)

Example1

```
// presigned url for 'getObject' method.
// expires in a day.
const presignedUrl = await minioClient.presignedUrl('GET', 'mybucket', 'hello.txt', 24 * 60 * 60)
console.log(presignedUrl)
```

Example2

```
// presigned url for 'listObject' method.
// Lists objects in 'myBucket' with prefix 'data'.
// Lists max 1000 of them.
await minioClient.presignedUrl('GET', 'mybucket', '', 1000, { prefix: 'data', 'max-keys': 1000 })
```

Example 3

```
// Get Object with versionid
await minioClient.presignedUrl('GET', 'mybucket', '', 1000, { versionId: '10fa9946-3f64-4137-a58f-888065c0732e' })
```

### presignedGetObject(bucketName, objectName[, expiry, respHeaders, requestDate])

Generates a presigned URL for HTTP GET operations. Browsers/Mobile clients may point to this URL to directly download objects even if the bucket is private. This presigned URL can have an associated expiration time in seconds after which the URL is no longer valid. The default value is 7 days.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`expiry`

number

Expiry time in seconds. Default value is 7 days. (optional)

`respHeaders`

object

response headers to override (optional)

`requestDate`

Date

A date object, the url will be issued at. Default value is now. (optional)

Example

```
// expires in a day.
const presignedUrl = await minioClient.presignedGetObject('mybucket', 'hello.txt', 24 * 60 * 60)
console.log(presignedUrl)
```

### presignedPutObject(bucketName, objectName [,expiry])

Generates a presigned URL for HTTP PUT operations. Browsers/Mobile clients may point to this URL to upload objects directly to a bucket even if it is private. This presigned URL can have an associated expiration time in seconds after which the URL is no longer valid. The default value is 7 days.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`objectName`

string

Name of the object.

`expiry`

number

Expiry time in seconds. Default value is 7 days.

Example

```
// expires in a day.
const presignedUrl = await minioClient.presignedPutObject('mybucket', 'hello.txt', 24 * 60 * 60)
console.log(presignedUrl)
```

### presignedPostPolicy(policy)

Allows setting policy conditions to a presigned URL for POST operations. Policies such as bucket name to receive object uploads, key name prefixes, expiry policy may be set.

Parameters

Param

Type

Description

`policy`

object

Policy object created by minioClient.newPostPolicy()

Create policy:

```
const policy = minioClient.newPostPolicy()
```

Apply upload policy restrictions:

```
// Policy restricted only for bucket 'mybucket'.
policy.setBucket('mybucket')

// Policy restricted only for hello.txt object.
policy.setKey('hello.txt')
```

or

```
// Policy restricted for incoming objects with keyPrefix.
policy.setKeyStartsWith('keyPrefix')

const expires = new Date()
expires.setSeconds(24 * 60 * 60 * 10)
// Policy expires in 10 days.
policy.setExpires(expires)

// Only allow 'text'.
policy.setContentType('text/plain')

// Set content disposition response header.
policy.setContentDisposition('attachment; filename=text.txt')

// Only allow content size in range 1KB to 1MB.
policy.setContentLengthRange(1024, 1024 * 1024)

// Set key-value user defined metadata
policy.setUserMetaData({
  key: 'value',
})
```

POST your content from the browser using `superagent`:

```
const { postURL, formData } = await minioClient.presignedPostPolicy(policy)
const req = superagent.post(postURL)
_.each(formData, function (value, key) {
  req.field(key, value)
})

// file contents.
req.attach('file', '/path/to/hello.txt', 'hello.txt')

req.end(function (err, res) {
  if (err) {
    return console.log(err.toString())
  }
  console.log('Upload successful.')
})
```

## 5. Bucket Policy & Notification operations

Buckets are configured to trigger notifications on specified types of events and paths filters.

### getBucketNotification(bucketName[, cb])

Fetch the notification configuration stored in the S3 provider and that belongs to the specified bucket name.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`callback(err, bucketNotificationConfig)`

function

Callback function is called with non `null` err value in case of error. `bucketNotificationConfig` will be the object that carries all notification configurations associated to bucketName. If no callback is passed, a `Promise` is returned.

Example

```
minioClient.getBucketNotification('mybucket', function (err, bucketNotificationConfig) {
  if (err) return console.log(err)
  console.log(bucketNotificationConfig)
})
```

### setBucketNotification(bucketName, bucketNotificationConfig[, callback])

Upload a user-created notification configuration and associate it to the specified bucket name.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`bucketNotificationConfig`

BucketNotification

Javascript object that carries the notification configuration.

`callback(err)`

function

Callback function is called with non `null` err value in case of error. If no callback is passed, a `Promise` is returned.

Example

```
// Create a new notification object
const bucketNotification = new Minio.NotificationConfig()

// Setup a new Queue configuration
const arn = Minio.buildARN('aws', 'sqs', 'us-west-2', '1', 'webhook')
const queue = new Minio.QueueConfig(arn)
queue.addFilterSuffix('.jpg')
queue.addFilterPrefix('myphotos/')
queue.addEvent(Minio.ObjectReducedRedundancyLostObject)
queue.addEvent(Minio.ObjectCreatedAll)

// Add the queue to the overall notification object
bucketNotification.add(queue)

minioClient.setBucketNotification('mybucket', bucketNotification, function (err) {
  if (err) return console.log(err)
  console.log('Success')
})
```

### removeAllBucketNotification(bucketName[, callback])

Remove the bucket notification configuration associated to the specified bucket.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket

`callback(err)`

function

Callback function is called with non `null` err value in case of error. If no callback is passed, a `Promise` is returned.

```
minioClient.removeAllBucketNotification('my-bucketname', function (e) {
  if (e) {
    return console.log(e)
  }
  console.log('True')
})
```

### listenBucketNotification(bucketName, prefix, suffix, events)

Listen for notifications on a bucket. Additionally one can provider
filters for prefix, suffix and events. There is no prior set bucket notification
needed to use this API. This is an MinIO extension API where unique identifiers
are registered and unregistered by the server automatically based on incoming requests.

Returns an `EventEmitter`, which will emit a `notification` event carrying the record.

To stop listening, call `.stop()` on the returned `EventEmitter`.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket

`prefix`

string

Object key prefix to filter notifications for.

`suffix`

string

Object key suffix to filter notifications for.

`events`

Array

Enables notifications for specific event types.

See [here](https://github.com/minio/minio-js/blob/master/examples/minio/listen-bucket-notification.js) for a full example.

```
const listener = minioClient.listenBucketNotification('my-bucketname', 'photos/', '.jpg', ['s3:ObjectCreated:*'])
listener.on('notification', function (record) {
  // For example: 's3:ObjectCreated:Put event occurred (2016-08-23T18:26:07.214Z)'
  console.log('%s event occurred (%s)', record.eventName, record.eventTime)
  listener.stop()
})
```

### async getBucketPolicy(bucketName: string): Promise

Get the bucket policy associated with the specified bucket. If `objectPrefix`
is not empty, the bucket policy will be filtered based on object permissions
as well.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket

```
// Retrieve bucket policy of 'my-bucketname'
const policy = await minioClient.getBucketPolicy('my-bucketname')

console.log(`Bucket policy file: ${policy}`)
```

### async setBucketPolicy(bucketName, bucketPolicy): Promise

Set the bucket policy on the specified bucket. [bucketPolicy](https://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html) is detailed here.

Parameters

Param

Type

Description

`bucketName`

string

Name of the bucket.

`bucketPolicy`

string

bucket policy.

```
// Set the bucket policy of `my-bucketname`
await minioClient.setBucketPolicy('my-bucketname', JSON.stringify(policy))
```

## 6. Custom Settings

### setS3TransferAccelerate(endpoint)

Set AWS S3 transfer acceleration endpoint for all API requests hereafter.
NOTE: This API applies only to AWS S3 and is a no operation for S3 compatible object storage services.

Parameters

Param

Type

Description

`endpoint`

string

Set to new S3 transfer acceleration endpoint.

## 7. HTTP request options

### setRequestOptions(options)

Set the HTTP/HTTPS request options. Supported options are `agent` ([http.Agent()](https://nodejs.org/api/http.html#http_class_http_agent)), `family` ([IP address family to use while resolving `host` or `hostname`](https://nodejs.org/api/http.html#http_http_request_url_options_callback)), and tls related options (‘agent’, ‘ca’, ‘cert’, ‘ciphers’, ‘clientCertEngine’, ‘crl’, ‘dhparam’, ‘ecdhCurve’, ‘honorCipherOrder’, ‘key’, ‘passphrase’, ‘pfx’, ‘rejectUnauthorized’, ‘secureOptions’, ‘secureProtocol’, ‘servername’, ‘sessionIdContext’) documented [here](https://nodejs.org/api/tls.html#tls_tls_createsecurecontext_options)

```
// Do not reject self signed certificates.
minioClient.setRequestOptions({ rejectUnauthorized: false })
```

## 7. Explore Further

[Build your own Shopping App Example](https://github.com/minio/minio-js-store-app)

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).