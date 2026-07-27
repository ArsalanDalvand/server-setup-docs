---
title: "Haskell Quickstart Guide — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/developers/haskell/minio-haskell.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "developers/haskell/minio-haskell"
category: "developers"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Haskell Quickstart Guide — SILO (MinIO Object Storage)"
---
# Haskell Quickstart Guide

## MinIO Haskell Client SDK for Amazon S3 Compatible Cloud Storage

The MinIO Haskell Client SDK provides simple APIs to access [MinIO](https://min.io) and any Amazon S3 compatible object storage.

This guide assumes that you have a working [Haskell development environment](https://www.haskell.org/downloads/).

### Installation

#### Add to your project

Simply add `minio-hs` to your project’s `.cabal` dependencies section or if you are using hpack, to your `package.yaml` file as usual.

#### Try it out in a REPL

##### For a cabal based environment

Download the library source and change to the extracted directory:

```
$ cabal get minio-hs
$ cd minio-hs-1.6.0/ # directory name could be different
```

Then load the `ghci` REPL environment with the library and browse the available APIs:

```
$ cabal repl
ghci> :browse Network.Minio
```

##### For a stack based environment

From your home folder or any non-haskell project directory, just run:

```
stack install minio-hs
```

Then start an interpreter session and browse the available APIs with:

```
$ stack ghci
> :browse Network.Minio
```

### Examples

The [examples](https://github.com/minio/minio-hs/tree/master/examples) folder contains many examples that you can try out and use to learn and to help with developing your own projects.

#### Quick-Start Example - File Uploader

This example program connects to a MinIO object storage server, makes a bucket on the server and then uploads a file to the bucket.

We will use the MinIO server running at https://play.min.io in this example. Feel free to use this service for testing and development. Access credentials are present in the library and are open to the public.

#### FileUploader.hs

```
#!/usr/bin/env stack
-- stack --resolver lts-14.11 runghc --package minio-hs --package optparse-applicative --package filepath

--
-- MinIO Haskell SDK, (C) 2017-2019 MinIO, Inc.
--
-- Licensed under the Apache License, Version 2.0 (the "License");
-- you may not use this file except in compliance with the License.
-- You may obtain a copy of the License at
--
--     http://www.apache.org/licenses/LICENSE-2.0
--
-- Unless required by applicable law or agreed to in writing, software
-- distributed under the License is distributed on an "AS IS" BASIS,
-- WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
-- See the License for the specific language governing permissions and
-- limitations under the License.
--

{-# LANGUAGE OverloadedStrings   #-}
{-# LANGUAGE ScopedTypeVariables #-}
import           Network.Minio

import           Data.Monoid           ((<>))
import           Data.Text             (pack)
import           Options.Applicative
import           System.FilePath.Posix
import           UnliftIO              (throwIO, try)

import           Prelude

-- | The following example uses minio's play server at
-- https://play.min.io.  The endpoint and associated
-- credentials are provided via the libary constant,
--
-- > minioPlayCI :: ConnectInfo
--

-- optparse-applicative package based command-line parsing.
fileNameArgs :: Parser FilePath
fileNameArgs = strArgument
               (metavar "FILENAME"
                <> help "Name of file to upload to AWS S3 or a MinIO server")

cmdParser = info
            (helper <*> fileNameArgs)
            (fullDesc
             <> progDesc "FileUploader"
             <> header
             "FileUploader - a simple file-uploader program using minio-hs")

main :: IO ()
main = do
  let bucket = "my-bucket"

  -- Parse command line argument
  filepath <- execParser cmdParser
  let object = pack $ takeBaseName filepath

  res <- runMinio minioPlayCI $ do
    -- Make a bucket; catch bucket already exists exception if thrown.
    bErr <- try $ makeBucket bucket Nothing

    -- If the bucket already exists, we would get a specific
    -- `ServiceErr` exception thrown.
    case bErr of
      Left BucketAlreadyOwnedByYou -> return ()
      Left e                       -> throwIO e
      Right _                      -> return ()

    -- Upload filepath to bucket; object name is derived from filepath.
    fPutObject bucket object filepath defaultPutObjectOptions

  case res of
    Left e   -> putStrLn $ "file upload failed due to " ++ show e
    Right () -> putStrLn "file upload succeeded."
```

#### Run FileUploader

```
./FileUploader.hs "path/to/my/file"
```

### Contribute

[Contributors Guide](https://github.com/minio/minio-hs/blob/master/CONTRIBUTING.md)

#### Development

##### Download the source

```
$ git clone https://github.com/minio/minio-hs.git
$ cd minio-hs/
```

##### Build the package:

With `cabal`:

```
$ # Configure cabal for development enabling all optional flags defined by the package.
$ cabal configure --enable-tests --test-show-details=direct -fexamples -fdev -flive-test
$ cabal build
```

With `stack`:

```
$ stack build --test --no-run-tests --flag minio-hs:live-test --flag minio-hs:dev --flag minio-hs:examples
```

##### Running tests:

A section of the tests use the remote MinIO Play server at `https://play.min.io` by default. For library development, using this remote server maybe slow. To run the tests against a locally running MinIO live server at `http://localhost:9000` with the credentials `access_key=minio` and `secret_key=minio123`, just set the environment `MINIO_LOCAL` to any value (and unset it to switch back to Play).

With `cabal`:

```
$ export MINIO_LOCAL=1 # to run live tests against local MinIO server
$ cabal test
```

With `stack`:

```
$ export MINIO_LOCAL=1 # to run live tests against local MinIO server
stack test --flag minio-hs:live-test --flag minio-hs:dev
```

This will run all the test suites.

##### Building documentation:

```
$ cabal haddock
$ # OR
$ stack haddock
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).