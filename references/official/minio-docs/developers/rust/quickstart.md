---
title: "MinIO Rust SDK for Amazon S3 Compatible Cloud Storage — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/developers/rust/quickstart.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "developers/rust/quickstart"
category: "developers"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Rust SDK for Amazon S3 Compatible Cloud Storage — SILO (MinIO Object Storage)"
---
# MinIO Rust SDK for Amazon S3 Compatible Cloud Storage

[](https://github.com/minio/minio-rs/actions/workflows/rust.yml)
[](https://docs.rs/minio/latest/minio/)
[](https://slack.min.io)
[](https://sourcegraph.com/github.com/minio/minio-rs?badge)
[](https://crates.io/crates/minio)
[](https://github.com/minio/minio-rs/blob/master/LICENSE)

The MinIO Rust SDK is a Simple Storage Service (aka S3) client for performing bucket and object operations to any Amazon S3 compatible object storage service.
It provides a strongly-typed, async-first interface to the MinIO and Amazon S3-compatible object storage APIs.

Each supported S3 operation has a corresponding request builder (for example: [`BucketExists`], [`PutObject`], [`UploadPartCopy`]), which allows users to configure request parameters using a fluent builder pattern.

All request builders implement the [`S3Api`] trait, which provides the async [`send`](#crate::s3::types::S3Api::send) method to execute the request and return a typed response.

## Basic Usage

```
use minio::s3::MinioClient;
use minio::s3::creds::StaticProvider;
use minio::s3::http::BaseUrl;
use minio::s3::types::S3Api;
use minio::s3::response::BucketExistsResponse;

#[tokio::main]
async fn main() {
    let base_url = "http://localhost:9000".parse::<BaseUrl>().unwrap();
    let static_provider = StaticProvider::new("minioadmin", "minioadmin", None);
    let client = MinioClient::new(base_url, Some(static_provider), None, None).unwrap();

    let exists: BucketExistsResponse = client
        .bucket_exists("my-bucket")
        .send()
        .await
        .expect("request failed");

    println!("Bucket exists: {}", exists.exists);
}
```

## Features

Request builder pattern for ergonomic API usage

Full async/await support via [`tokio`]

Strongly-typed responses

Transparent error handling via `Result<T, Error>`

## Design

Each API method on the [`MinioClient`] returns a builder struct

Builders implement [`ToS3Request`] for request conversion and [`S3Api`] for execution

Responses implement [`FromS3Response`] for consistent deserialization

## Examples

You can run the examples from the command line with:

`cargo run --example <example_name>`

The examples below cover several common operations.
You can find the complete list of examples in the `examples` directory.

### file_uploader.rs

[Upload a file to MinIO](#examples/file_uploader.rs)

[Upload a file to MinIO with CLI](#examples/put_object.rs)

### file_downloader.rs

[Download a file from MinIO](#examples/file_downloader.rs)

### object_prompt.rs

[Prompt a file on MinIO](#examples/object_prompt.rs)

## License

This SDK is distributed under the [Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0), see [LICENSE](https://github.com/minio/minio-rs/blob/master/LICENSE) for more information.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).