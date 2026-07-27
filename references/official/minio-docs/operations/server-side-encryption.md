---
title: "Data Encryption (SSE) — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/server-side-encryption.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/server-side-encryption"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Data Encryption (SSE) — SILO (MinIO Object Storage)"
---
# Data Encryption (SSE)

MinIO Server-Side Encryption (SSE) protects objects as part of write operations,
allowing clients to take advantage of server processing power to secure objects
at the storage layer (encryption-at-rest). SSE also provides key functionality
to regulatory and compliance requirements around secure locking and erasure.

MinIO SSE uses the [MinIO Key Encryption Service (KES)](https://github.com/minio/kes) and an
external Key Management Service (KMS) for performing secured cryptographic
operations at scale. MinIO also supports client-managed key management, where
the application takes full responsibility for creating and managing encryption
keys for use with MinIO SSE.

MinIO supports the following KMS as the central key store:

[[operations/server-side-encryption/configure-minio-kes#minio-sse-vault|HashiCorp KeyVault]]

[[operations/server-side-encryption/configure-minio-kes#minio-sse-aws|AWS SecretsManager]]

[[operations/server-side-encryption/configure-minio-kes#minio-sse-gcp|Google Cloud SecretManager]]

[[operations/server-side-encryption/configure-minio-kes#minio-sse-azure|Azure Key Vault]]

[Fortanix SDKMS](https://github.com/minio/kes/wiki/Fortanix-SDKMS)

[Thales Digital Identity and Security (formerly Gemalto)](https://github.com/minio/kes/wiki/Gemalto-KeySecure)

MinIO SSE requires enabling [[operations/network-encryption#minio-tls|Network Encryption (TLS)]].

## Supported Encryption Types

MinIO SSE is feature and API compatible with
[AWS Server-Side Encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/server-side-encryption.html) and
supports the following encryption strategies:

SSE-KMS Recommended

MinIO supports enabling automatic SSE-KMS encryption of all objects
written to a bucket using a specific External Key (EK) stored on the
external KMS. Clients can override the bucket-default EK by specifying
an explicit key as part of the write operation.

For buckets without automatic SSE-KMS encryption, clients can specify
an EK as part of the write operation instead.

MinIO encrypts backend data as part of enabling server-side encryption.
You cannot disable SSE-KMS encryption once enabled.

SSE-KMS provides more granular and customizable encryption compared to
SSE-S3 and SSE-C and is recommended over the other supported encryption
methods.

For a tutorial on enabling SSE-KMS in a local (non-production) MinIO Deployment, see [[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms-quickstart|Quickstart]].

SSE-S3

MinIO supports enabling automatic SSE-S3 encryption of all objects
written to a bucket using an EK stored on the external KMS. MinIO
SSE-S3 supports one EK for the entire deployment.

For buckets without automatic SSE-S3 encryption, clients can request
SSE encryption as part of the write operation instead.

MinIO encrypts backend data as part of enabling server-side encryption.
You cannot disable SSE-KMS encryption once enabled.

For a tutorial on enabling SSE-s3 in a local (non-production) MinIO Deployment, see [[administration/server-side-encryption/server-side-encryption-sse-s3#minio-encryption-sse-s3-quickstart|Quickstart]].

SSE-C

Clients specify an EK as part of the write operation for an object.
MinIO uses the specified EK to perform SSE-S3.

SSE-C does not support bucket-default encryption settings and requires
clients perform all key management operations.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).