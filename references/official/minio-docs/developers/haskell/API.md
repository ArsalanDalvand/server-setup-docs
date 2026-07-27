---
title: "MinIO Haskell SDK API Reference — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/developers/haskell/API.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "developers/haskell/API"
category: "developers"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Haskell SDK API Reference — SILO (MinIO Object Storage)"
---
# MinIO Haskell SDK API Reference

## Initialize MinIO Client object.

### MinIO - for public Play server

```
minioPlayCI :: ConnectInfo
minioPlayCI
```

### AWS S3

```
awsCI :: ConnectInfo
awsCI { connectAccesskey = "your-access-key"
      , connectSecretkey = "your-secret-key"
      }
```

Bucket operations

Object Operations

Presigned Operations

[`listBuckets`](#listBuckets)

[`getObject`](#getObject)

[`presignedGetObjectUrl`](#presignedGetObjectUrl)

[`makeBucket`](#makeBucket)

[`putObject`](#putObject)

[`presignedPutObjectUrl`](#presignedPutObjectUrl)

[`removeBucket`](#removeBucket)

[`fGetObject`](#fGetObject)

[`presignedPostPolicy`](#presignedPostPolicy)

[`listObjects`](#listObjects)

[`fPutObject`](#fPutObject)

[`listObjectsV1`](#listObjectsV1)

[`copyObject`](#copyObject)

[`listIncompleteUploads`](#listIncompleteUploads)

[`removeObject`](#removeObject)

[`bucketExists`](#bucketExists)

[`selectObjectContent`](#selectObjectContent)

## 1. Connecting and running operations on the storage service

The Haskell MinIO SDK provides high-level functionality to perform
operations on a MinIO server or any AWS S3-like API compatible storage
service.

### The ConnectInfo type

The `ConnectInfo` record-type contains connection information for a
particular server. It is recommended to construct the `ConnectInfo`
value using one of the several smart constructors provided by the
library, documented in the following subsections.

The library automatically discovers the region of a bucket by
default. This is especially useful with AWS, where buckets may be in
different regions. When performing an upload, download or other
operation, the library requests the service for the location of a
bucket and caches it for subsequent requests.

#### awsCI :: ConnectInfo

`awsCI` is a value that provides connection information for AWS
S3. Credentials can be supplied by overriding a couple of fields like
so:

```
awsConn = awsCI {
    connectAccessKey = "my-AWS-access-key"
  , connectSecretKey = "my-AWS-secret-key"
  }
```

#### awsWithRegionCI :: Region -> Bool -> ConnectInfo

This constructor allows to specify the initial region and a Boolean to
enable/disable the automatic region discovery behaviour.

The parameters in the expression `awsWithRegion region autoDiscover` are:

Parameter

Type

Description

`region`

Region (alias for `Text`)

The region to connect to by default for all requests.

`autoDiscover`

Bool

If `True`, region discovery will be enabled. If `False`, discovery is disabled, and all requests go the given region only.

#### minioPlayCI :: ConnectInfo

This constructor provides connection and authentication information to
connect to the public MinIO Play server at
`https://play.min.io/`.

#### minioCI :: Text -> Int -> Bool -> ConnectInfo

Use to connect to a MinIO server.

The parameters in the expression `minioCI host port isSecure` are:

Parameter

Type

Description

`host`

Text

Hostname of the MinIO or other S3-API compatible server

`port`

Int

Port number to connect to

`isSecure`

Bool

Does the server use HTTPS?

#### The ConnectInfo fields and Default instance

The following table shows the fields in the `ConnectInfo` record-type:

Field

Type

Description

`connectHost`

Text

Host name of the server. Defaults to `localhost`.

`connectPort`

Int

Port number on which the server listens. Defaults to `9000`.

`connectAccessKey`

Text

Access key to use in authentication. Defaults to `minio`.

`connectSecretkey`

Text

Secret key to use in authentication. Defaults to `minio123`.

`connectIsSecure`

Bool

Specifies if the server used TLS. Defaults to `False`

`connectRegion`

Region (alias for `Text`)

Specifies the region to use. Defaults to ‘us-east-1’

`connectAutoDiscoverRegion`

Bool

Specifies if the library should automatically discover the region of a bucket. Defaults to `True`

The `def` value of type `ConnectInfo` has all the above default
values.

### The Minio Monad

This monad provides the required environment to perform requests
against a MinIO or other S3 API compatible server. It uses the
connection information from the `ConnectInfo` value provided to it. It
performs connection pooling, bucket location caching, error handling
and resource clean-up actions.

The `runMinio` function performs the provided action in the `Minio`
monad and returns a `IO (Either MinioErr a)` value:

```
{-# Language OverloadedStrings #-}

import Network.Minio

main :: IO ()
main = do
  result <- runMinio def $ do
    buckets <- listBuckets
    return $ length buckets

  case result of
    Left e -> putStrLn $ "Failed operation with error: " ++ show e
    Right n -> putStrLn $ show n ++ " bucket(s) found."
```

The above performs a `listBuckets` operation and returns the number of
buckets in the server. If there were any errors, they will be returned
as values of type `MinioErr` as a `Left` value.

## 2. Bucket operations

### listBuckets :: Minio [BucketInfo]

Lists buckets.

Return Value

Return type

Description

Minio [BucketInfo]

List of buckets

BucketInfo record type

Field

Type

Description

`biName`

Bucket (alias of `Text`)

Name of the bucket

`biCreationDate`

UTCTime

Creation time of the bucket

### makeBucket :: Bucket -> Maybe Region -> Minio ()

Create a new bucket. If the region is not specified, the region
specified by `ConnectInfo` is used.

Parameters

In the expression `makeBucket bucketName region` the arguments are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`region`

Maybe Region

Region where the bucket is to be created. If not specified, default to the region in `ConnectInfo`.

Example

```
{-# Language OverloadedStrings #-}

main :: IO ()
main = do
    res <- runMinio minioPlayCI $ do
        makeBucket bucketName (Just "us-east-1")
    case res of
        Left err -> putStrLn $ "Failed to make bucket: " ++ (show res)
        Right _ -> putStrLn $ "makeBucket successful."
```

### removeBucket :: Bucket -> Minio ()

Remove a bucket. The bucket must be empty or an error will be thrown.

Parameters

In the expression `removeBucket bucketName` the arguments are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

Example

```
{-# Language OverloadedStrings #-}

main :: IO ()
main = do
    res <- runMinio minioPlayCI $ do
        removeBucket "mybucket"

    case res of
        Left err -> putStrLn $ "Failed to remove bucket: " ++ (show res)
        Right _ -> putStrLn $ "removeBucket successful."
```

### listObjects :: Bucket -> Maybe Text -> Bool -> C.ConduitM () ObjectInfo Minio ()

List objects in the given bucket, implements version 2 of AWS S3 API.

Parameters

In the expression `listObjects bucketName prefix recursive` the
arguments are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`prefix`

Maybe Text

Optional prefix that listed objects should have

`recursive`

Bool

`True` indicates recursive style listing and `False` indicates directory style listing delimited by ‘/’.

Return Value

Return type

Description

C.ConduitM () ObjectInfo Minio ()

A Conduit Producer of `ObjectInfo` values corresponding to each object.

ObjectInfo record type

Field

Type

Description

`oiObject`

Object (alias for `Text`)

Name of object

`oiModTime`

UTCTime

Last modified time of the object

`oiETag`

ETag (alias for `Text`)

ETag of the object

`oiSize`

Int64

Size of the object in bytes

`oiMetadata`

HashMap Text Text

Map of key-value user-metadata pairs

Example

```
{-# LANGUAGE OverloadedStrings #-}
import           Network.Minio

import           Conduit
import           Prelude

-- | The following example uses MinIO play server at
-- https://play.min.io.  The endpoint and associated
-- credentials are provided via the libary constant,
--
-- > minioPlayCI :: ConnectInfo
--

main :: IO ()
main = do
  let
    bucket = "test"

  -- Performs a recursive listing of all objects under bucket "test"
  -- on play.min.io.
  res <- runMinio minioPlayCI $
    runConduit $ listObjects bucket Nothing True .| mapM_C (\v -> (liftIO $ print v))
  print res
```

### listObjectsV1 :: Bucket -> Maybe Text -> Bool -> C.ConduitM () ObjectInfo Minio ()

List objects in the given bucket, implements version 1 of AWS S3 API. This API
is provided for legacy S3 compatible object storage endpoints.

Parameters

In the expression `listObjectsV1 bucketName prefix recursive` the
arguments are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`prefix`

Maybe Text

Optional prefix that listed objects should have

`recursive`

Bool

`True` indicates recursive style listing and `False` indicates directory style listing delimited by ‘/’.

Return Value

Return type

Description

C.ConduitM () ObjectInfo Minio ()

A Conduit Producer of `ObjectInfo` values corresponding to each object.

ObjectInfo record type

Field

Type

Description

`oiObject`

Object (alias for `Text`)

Name of object

`oiModTime`

UTCTime

Last modified time of the object

`oiETag`

ETag (alias for `Text`)

ETag of the object

`oiSize`

Int64

Size of the object in bytes

Example

```
{-# LANGUAGE OverloadedStrings #-}
import           Network.Minio

import           Conduit
import           Prelude

-- | The following example uses MinIO play server at
-- https://play.min.io.  The endpoint and associated
-- credentials are provided via the libary constant,
--
-- > minioPlayCI :: ConnectInfo
--

main :: IO ()
main = do
  let
    bucket = "test"

  -- Performs a recursive listing of all objects under bucket "test"
  -- on play.min.io.
  res <- runMinio minioPlayCI $
    runConduit $ listObjectsV1 bucket Nothing True .| mapM_C (\v -> (liftIO $ print v))
  print res
```

### listIncompleteUploads :: Bucket -> Maybe Prefix -> Bool -> C.Producer Minio UploadInfo

List incompletely uploaded objects.

Parameters

In the expression `listIncompleteUploads bucketName prefix recursive`
the parameters are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`prefix`

Maybe Text

Optional prefix that listed objects should have.

`recursive`

Bool

`True` indicates recursive style listing and `Talse` indicates directory style listing delimited by ‘/’.

Return Value

Return type

Description

C.ConduitM () UploadInfo Minio ()

A Conduit Producer of `UploadInfo` values corresponding to each incomplete multipart upload

UploadInfo record type

Field

Type

Description

`uiKey`

Object

Name of incompletely uploaded object

`uiUploadId`

String

Upload ID of incompletely uploaded object

`uiSize`

Int64

Size of incompletely uploaded object

Example

```
{-# LANGUAGE OverloadedStrings #-}
import           Network.Minio

import           Conduit
import           Prelude

-- | The following example uses MinIO play server at
-- https://play.min.io.  The endpoint and associated
-- credentials are provided via the libary constant,
--
-- > minioPlayCI :: ConnectInfo
--

main :: IO ()
main = do
  let
    bucket = "test"

  -- Performs a recursive listing of incomplete uploads under bucket "test"
  -- on a local MinIO server.
  res <- runMinio minioPlayCI $
    runConduit $ listIncompleteUploads bucket Nothing True .| mapM_C (\v -> (liftIO $ print v))
  print res
```

## 3. Object operations

### getObject :: Bucket -> Object -> GetObjectOptions -> Minio (C.ConduitM () ByteString Minio ())

Get an object from the S3 service, optionally object ranges can be provided as well.

Parameters

In the expression `getObject bucketName objectName opts` the parameters
are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`objectName`

Object (alias for `Text`)

Name of the object

`opts`

GetObjectOptions

Options for GET requests specifying additional options like If-Match, Range

GetObjectOptions record type

Field

Type

Description

`gooRange`

`Maybe ByteRanges`

Represents the byte range of object. E.g ByteRangeFromTo 0 9 represents first ten bytes of the object

`gooIfMatch`

`Maybe ETag` (alias for `Text`)

(Optional) ETag of object should match

`gooIfNoneMatch`

`Maybe ETag` (alias for `Text`)

(Optional) ETag of object shouldn’t match

`gooIfUnmodifiedSince`

`Maybe UTCTime`

(Optional) Time since object wasn’t modified

`gooIfModifiedSince`

`Maybe UTCTime`

(Optional) Time since object was modified

Return Value

The return value can be incrementally read to process the contents of
the object.

Return type

Description

Minio (C.ConduitM () ByteString Minio ())

A Conduit source of `ByteString` values.

Example

```
{-# LANGUAGE OverloadedStrings #-}
import           Network.Minio

import qualified Data.Conduit        as C
import qualified Data.Conduit.Binary as CB

import           Prelude

-- | The following example uses MinIO play server at
-- https://play.min.io.  The endpoint and associated
-- credentials are provided via the libary constant,
--
-- > minioPlayCI :: ConnectInfo
--

main :: IO ()
main = do
  let
      bucket = "my-bucket"
      object = "my-object"
  res <- runMinio minioPlayCI $ do
    src <- getObject bucket object def
    C.connect src $ CB.sinkFileCautious "/tmp/my-object"

  case res of
    Left e  -> putStrLn $ "getObject failed." ++ (show e)
    Right _ -> putStrLn "getObject succeeded."
```

### putObject :: Bucket -> Object -> C.ConduitM () ByteString Minio () -> Maybe Int64 -> PutObjectOptions -> Minio ()

Uploads an object to a bucket in the service, from the given input
byte stream of optionally supplied length. Optionally you can also specify
additional metadata for the object.

Parameters

In the expression `putObject bucketName objectName inputSrc` the parameters
are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`objectName`

Object (alias for `Text`)

Name of the object

`inputSrc`

C.ConduitM () ByteString Minio ()

A Conduit producer of `ByteString` values

`size`

Int64

Provide stream size (optional)

`opts`

PutObjectOptions

Optional parameters to provide additional metadata for the object

Example

```
{-# LANGUAGE OverloadedStrings #-}
import           Network.Minio

import qualified Data.Conduit.Combinators as CC

import           Prelude

-- | The following example uses MinIO play server at
-- https://play.min.io.  The endpoint and associated
-- credentials are provided via the libary constant,
--
-- > minioPlayCI :: ConnectInfo
--

main :: IO ()
main = do
  let
      bucket = "test"
      object = "obj"
      localFile = "/etc/lsb-release"
      kb15 = 15 * 1024

  -- Eg 1. Upload a stream of repeating "a" using putObject with default options.
  res <- runMinio minioPlayCI $
    putObject bucket object (CC.repeat "a") (Just kb15) def
  case res of
    Left e   -> putStrLn $ "putObject failed." ++ show e
    Right () -> putStrLn "putObject succeeded."
```

### fGetObject :: Bucket -> Object -> FilePath -> GetObjectOptions -> Minio ()

Downloads an object from a bucket in the service, to the given file

Parameters

In the expression `fGetObject bucketName objectName inputFile` the parameters
are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`objectName`

Object (alias for `Text`)

Name of the object

`inputFile`

FilePath

Path to the file to be uploaded

`opts`

GetObjectOptions

Options for GET requests specifying additional options like If-Match, Range

GetObjectOptions record type

Field

Type

Description

`gooRange`

`Maybe ByteRanges`

Represents the byte range of object. E.g ByteRangeFromTo 0 9 represents first ten bytes of the object

`gooIfMatch`

`Maybe ETag` (alias for `Text`)

(Optional) ETag of object should match

`gooIfNoneMatch`

`Maybe ETag` (alias for `Text`)

(Optional) ETag of object shouldn’t match

`gooIfUnmodifiedSince`

`Maybe UTCTime`

(Optional) Time since object wasn’t modified

`gooIfModifiedSince`

`Maybe UTCTime`

(Optional) Time since object was modified

```
{-# Language OverloadedStrings #-}
import Network.Minio

import Data.Conduit (($$+-))
import Data.Conduit.Binary (sinkLbs)
import Prelude

-- | The following example uses MinIO play server at
-- https://play.min.io.  The endpoint and associated
-- credentials are provided via the libary constant,
--
-- > minioPlayCI :: ConnectInfo
--

main :: IO ()
main = do
  let
      bucket = "my-bucket"
      object = "my-object"
      localFile = "/etc/lsb-release"

  res <- runMinio minioPlayCI $ do
    src <- fGetObject bucket object localFile def
    (src $$+- sinkLbs)

  case res of
    Left e -> putStrLn $ "fGetObject failed." ++ (show e)
    Right _ -> putStrLn "fGetObject succeeded."
```

### fPutObject :: Bucket -> Object -> FilePath -> Minio ()

Uploads an object to a bucket in the service, from the given file

Parameters

In the expression `fPutObject bucketName objectName inputFile` the parameters
are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`objectName`

Object (alias for `Text`)

Name of the object

`inputFile`

FilePath

Path to the file to be uploaded

Example

```
{-# Language OverloadedStrings #-}
import Network.Minio
import qualified Data.Conduit.Combinators as CC

main :: IO ()
main = do
  let
    bucket = "mybucket"
    object = "myobject"
    localFile = "/etc/lsb-release"

  res <- runMinio minioPlayCI $ do
           fPutObject bucket object localFile

  case res of
    Left e -> putStrLn $ "Failed to fPutObject " ++ show bucket ++ "/" ++ show object
    Right _ -> putStrLn "fPutObject was successful"
```

### copyObject :: DestinationInfo -> SourceInfo -> Minio ()

Copies content of an object from the service to another

Parameters

In the expression `copyObject dstInfo srcInfo` the parameters
are:

Param

Type

Description

`dstInfo`

DestinationInfo

A value representing properties of the destination object

`srcInfo`

SourceInfo

A value representing properties of the source object

SourceInfo record type

Field

Type

Description

`srcBucket`

`Bucket`

Name of source bucket

`srcObject`

`Object`

Name of source object

`srcRange`

`Maybe (Int64, Int64)`

(Optional) Represents the byte range of source object. (0, 9) represents first ten bytes of source object

`srcIfMatch`

`Maybe Text`

(Optional) ETag source object should match

`srcIfNoneMatch`

`Maybe Text`

(Optional) ETag source object shouldn’t match

`srcIfUnmodifiedSince`

`Maybe UTCTime`

(Optional) Time since source object wasn’t modified

`srcIfModifiedSince`

`Maybe UTCTime`

(Optional) Time since source object was modified

Destination record type

Field

Type

Description

`dstBucket`

`Bucket`

Name of destination bucket in server-side copyObject

`dstObject`

`Object`

Name of destination object in server-side copyObject

Example

```
{-# Language OverloadedStrings #-}
import Network.Minio

main :: IO ()
main = do
  let
    bucket = "mybucket"
    object = "myobject"
    objectCopy = "obj-copy"

  res <- runMinio minioPlayCI $ do
           copyObject def { dstBucket = bucket, dstObject = objectCopy } def { srcBucket = bucket, srcObject = object }

  case res of
    Left e -> putStrLn $ "Failed to copyObject " ++ show bucket ++ show "/" ++ show object
    Right _ -> putStrLn "copyObject was successful"
```

### removeObject :: Bucket -> Object -> Minio ()

Removes an object from the service

Parameters

In the expression `removeObject bucketName objectName` the parameters
are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`objectName`

Object (alias for `Text`)

Name of the object

Example

```
{-# Language OverloadedStrings #-}
import Network.Minio

main :: IO ()
main = do
  let
    bucket = "mybucket"
    object = "myobject"

  res <- runMinio minioPlayCI $ do
           removeObject bucket object

  case res of
    Left e -> putStrLn $ "Failed to remove " ++ show bucket ++ "/" ++ show object
    Right _ -> putStrLn "Removed object successfully"
```

### removeIncompleteUpload :: Bucket -> Object -> Minio ()

Removes an ongoing multipart upload of an object from the service

Parameters

In the expression `removeIncompleteUpload bucketName objectName` the parameters
are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`objectName`

Object (alias for `Text`)

Name of the object

Example

```
{-# Language OverloadedStrings #-}
import Network.Minio

main :: IO ()
main = do
  let
    bucket = "mybucket"
    object = "myobject"

  res <- runMinio minioPlayCI $
           removeIncompleteUpload bucket object

  case res of
    Left _ -> putStrLn $ "Failed to remove " ++ show bucket ++ "/" ++ show object
    Right _ -> putStrLn "Removed incomplete upload successfully"
```

### selectObjectContent :: Bucket -> Object -> SelectRequest -> Minio (ConduitT () EventMessage Minio ())

Removes an ongoing multipart upload of an object from the service

Parameters

In the expression `selectObjectContent bucketName objectName selReq`
the parameters are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`objectName`

Object (alias for `Text`)

Name of the object

`selReq`

SelectRequest

Select request parameters

SelectRequest record

This record is created using `selectRequest`. Please refer to the Haddocks for further information.

Return Value

The return value can be used to read individual `EventMessage`s in the response. Please refer to the Haddocks for further information.

Return type

Description

Minio (C.conduitT () EventMessage Minio ())

A Conduit source of `EventMessage` values.

Example

```
{-# Language OverloadedStrings #-}
import Network.Minio

import qualified Conduit              as C

main :: IO ()
main = do
  let
    bucket = "mybucket"
    object = "myobject"

  res <- runMinio minioPlayCI $ do
    let sr = selectRequest "Select * from s3object"
             defaultCsvInput defaultCsvOutput
    res <- selectObjectContent bucket object sr
    C.runConduit $ res C..| getPayloadBytes C..| C.stdoutC

  case res of
    Left _ -> putStrLn "Failed!"
    Right _ -> putStrLn "Success!"
```

### bucketExists :: Bucket -> Minio Bool

Checks if a bucket exists.

Parameters

In the expression `bucketExists bucketName` the parameters are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

## 4. Presigned operations

### presignedGetObjectUrl :: Bucket -> Object -> UrlExpiry -> Query -> RequestHeaders -> Minio ByteString

Generate a URL with authentication signature to GET (download) an
object. All extra query parameters and headers passed here will be
signed and are required when the generated URL is used. Query
parameters could be used to change the response headers sent by the
server. Headers can be used to set Etag match conditions among others.

For a list of possible request parameters and headers, please refer
to the GET object REST API AWS S3 documentation.

Parameters

In the expression `presignedGetObjectUrl bucketName objectName expiry queryParams headers`
the parameters are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`objectName`

Object (alias for `Text`)

Name of the object

`expiry`

UrlExpiry (alias for `Int`)

Url expiry time in seconds

`queryParams`

Query (from package `http-types:Network.HTTP.Types`)

Query parameters to add to the URL

`headers`

RequestHeaders (from package `http-types:Network.HTTP.Types`

Request headers that would be used with the URL

Return Value

Returns the generated URL - it will include authentication
information.

Return type

Description

ByteString

Generated presigned URL

Example

```
{-# Language OverloadedStrings #-}

import Network.Minio
import qualified Data.ByteString.Char8 as B

main :: IO ()
main = do
  let
    bucket = "mybucket"
    object = "myobject"

  res <- runMinio minioPlayCI $ do
           -- Set a 7 day expiry for the URL
           presignedGetObjectUrl bucket object (7*24*3600) [] []

  -- Print the URL on success.
  putStrLn $ either
    (("Failed to generate URL: " ++) . show)
    B.unpack
    res
```

### presignedPutObjectUrl :: Bucket -> Object -> UrlExpiry -> RequestHeaders -> Minio ByteString

Generate a URL with authentication signature to PUT (upload) an
object. Any extra headers if passed, are signed, and so they are
required when the URL is used to upload data. This could be used, for
example, to set user-metadata on the object.

For a list of possible headers to pass, please refer to the PUT object
REST API AWS S3 documentation.

Parameters

In the expression `presignedPutObjectUrl bucketName objectName expiry headers`
the parameters are:

Param

Type

Description

`bucketName`

Bucket (alias for `Text`)

Name of the bucket

`objectName`

Object (alias for `Text`)

Name of the object

`expiry`

UrlExpiry (alias for `Int`)

Url expiry time in seconds

`headers`

RequestHeaders (from package `http-types:Network.HTTP.Types`

Request headers that would be used with the URL

Return Value

Returns the generated URL - it will include authentication
information.

Return type

Description

ByteString

Generated presigned URL

Example

```
{-# Language OverloadedStrings #-}

import Network.Minio
import qualified Data.ByteString.Char8 as B

main :: IO ()
main = do
  let
    bucket = "mybucket"
    object = "myobject"

  res <- runMinio minioPlayCI $ do
           -- Set a 7 day expiry for the URL
           presignedPutObjectUrl bucket object (7*24*3600) [] []

  -- Print the URL on success.
  putStrLn $ either
    (("Failed to generate URL: " ++) . show)
    B.unpack
    res
```

### presignedPostPolicy :: PostPolicy -> Minio (ByteString, HashMap Text ByteString)

Generate a presigned URL and POST policy to upload files via a POST
request. This is intended for browser uploads and generates form data
that should be submitted in the request.

The `PostPolicy` argument is created using the `newPostPolicy` function:

#### newPostPolicy :: UTCTime -> [PostPolicyCondition] -> Either PostPolicyError PostPolicy

In the expression `newPostPolicy expirationTime conditions` the parameters are:

Param

Type

Description

`expirationTime`

UTCTime (from package `time:Data.Time.UTCTime`)

The expiration time for the policy

`conditions`

[PostPolicyConditions]

List of conditions to be added to the policy

The policy conditions are created using various helper functions -
please refer to the Haddocks for details.

Since conditions are validated by `newPostPolicy` it returns an
`Either` value.

Return Value

`presignedPostPolicy` returns a 2-tuple - the generated URL and a map
containing the form-data that should be submitted with the request.

Example

```
{-# Language OverloadedStrings #-}

import Network.Minio

import qualified Data.ByteString       as B
import qualified Data.ByteString.Char8 as Char8
import qualified Data.HashMap.Strict   as H
import qualified Data.Text.Encoding    as Enc
import qualified Data.Time             as Time

main :: IO ()
main = do
  now <- Time.getCurrentTime
  let
    bucket = "mybucket"
    object = "myobject"

    -- set an expiration time of 10 days
    expireTime = Time.addUTCTime (3600 * 24 * 10) now

    -- create a policy with expiration time and conditions - since the
    -- conditions are validated, newPostPolicy returns an Either value
    policyE = newPostPolicy expireTime
              [ -- set the object name condition
                ppCondKey "photos/my-object"
                -- set the bucket name condition
              , ppCondBucket "my-bucket"
                -- set the size range of object as 1B to 10MiB
              , ppCondContentLengthRange 1 (10*1024*1024)
                -- set content type as jpg image
              , ppCondContentType "image/jpeg"
                -- on success set the server response code to 200
              , ppCondSuccessActionStatus 200
              ]

  case policyE of
    Left err -> putStrLn $ show err
    Right policy -> do
      res <- runMinio minioPlayCI $ do
        (url, formData) <- presignedPostPolicy policy

        -- a curl command is output to demonstrate using the generated
        -- URL and form-data
        let
          formFn (k, v) = B.concat ["-F ", Enc.encodeUtf8 k, "=",
                                    "'", v, "'"]
          formOptions = B.intercalate " " $ map formFn $ H.toList formData

        return $ B.intercalate " " $
          ["curl", formOptions, "-F file=@/tmp/photo.jpg", url]

      case res of
        Left e -> putStrLn $ "post-policy error: " ++ (show e)
        Right cmd -> do
          putStrLn $ "Put a photo at /tmp/photo.jpg and run command:\n"

          -- print the generated curl command
          Char8.putStrLn cmd
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).