---
title: "Server-Side Encryption of Objects — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/server-side-encryption.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/server-side-encryption"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Server-Side Encryption of Objects — SILO (MinIO Object Storage)"
---
# Server-Side Encryption of Objects

MinIO Server-Side Encryption (SSE) protects objects as part of write operations, allowing clients to take advantage of server processing power to secure objects at the storage layer (encryption-at-rest).
SSE also provides key functionality to regulatory and compliance requirements around secure locking and erasure.

MinIO SSE uses the [MinIO Key Encryption Service (KES)](https://docs.min.io/community/minio-kes/) and an external Key Management Service (KMS) for performing secured cryptographic operations at scale.
MinIO also supports client-managed key management, where the application takes full responsibility for creating and managing encryption keys for use with MinIO SSE.

MinIO SSE is feature and API compatible with [AWS Server-Side Encryption](https://docs.aws.amazon.com/AmazonS3/latest/userguide/server-side-encryption.html) and supports the following encryption strategies:

SSE-KMS Recommended

MinIO supports enabling automatic SSE-KMS encryption of all objects written to a bucket using a specific External Key (EK) stored on the external KMS.
Clients can override the bucket-default EK by specifying an explicit key as part of the write operation.

For buckets without automatic SSE-KMS encryption, clients can specify an EK as part of the write operation instead.

MinIO encrypts backend data as part of enabling server-side encryption.
You cannot disable SSE-KMS encryption once enabled.

SSE-KMS provides more granular and customizable encryption compared to SSE-S3 and SSE-C and is recommended over the other supported encryption methods.

For a tutorial on enabling SSE-KMS in a local (non-production) MinIO Deployment, see [[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms-quickstart|Quickstart]].
For production MinIO deployments, use one of the following guides:

[AWS Secrets Manager](https://docs.min.io/community/minio-kes/integrations/aws-secrets-manager/)

[Azure Key Vault](https://docs.min.io/community/minio-kes/integrations/azure-keyvault/)

[Entrust KeyControl](https://docs.min.io/community/minio-kes/integrations/entrust-keycontrol/)

[Fortanix SDKMS](https://docs.min.io/community/minio-kes/integrations/fortanix-sdkms/)

[Google Cloud Secret Manager](https://docs.min.io/community/minio-kes/integrations/google-cloud-secret-manager/)

[HashiCorp Vault Keystore](https://docs.min.io/community/minio-kes/integrations/hashicorp-vault-keystore/)

[Thales CipherTrust Manager (formerly Gemalto KeySecure)](https://docs.min.io/community/minio-kes/integrations/thales-ciphertrust/)

SSE-S3

MinIO supports enabling automatic SSE-S3 encryption of all objects
written to a bucket using an EK stored on the external KMS. MinIO
SSE-S3 supports one EK for the entire deployment.

For buckets without automatic SSE-S3 encryption, clients can request
SSE encryption as part of the write operation instead.

MinIO encrypts backend data as part of enabling server-side encryption.
You cannot disable SSE-KMS encryption once enabled.

For a tutorial on enabling SSE-s3 in a local (non-production) MinIO
Deployment, see [[administration/server-side-encryption/server-side-encryption-sse-s3#minio-encryption-sse-s3-quickstart|Quickstart]]. For
production MinIO deployments, use one of the following guides:

[AWS Secrets Manager](https://docs.min.io/community/minio-kes/integrations/aws-secrets-manager/)

[Azure Key Vault](https://docs.min.io/community/minio-kes/integrations/azure-keyvault/)

[Entrust KeyControl](https://docs.min.io/community/minio-kes/integrations/entrust-keycontrol/)

[Fortanix SDKMS](https://docs.min.io/community/minio-kes/integrations/fortanix-sdkms/)

[Google Cloud Secret Manager](https://docs.min.io/community/minio-kes/integrations/google-cloud-secret-manager/)

[HashiCorp Vault Keystore](https://docs.min.io/community/minio-kes/integrations/hashicorp-vault-keystore/)

[Thales CipherTrust Manager (formerly Gemalto KeySecure)](https://docs.min.io/community/minio-kes/integrations/thales-ciphertrust/)

SSE-C

Clients specify an EK as part of the write operation for an object.
MinIO uses the specified EK to perform SSE-S3.

SSE-C does not support bucket-default encryption settings and requires
clients perform all key management operations.

MinIO SSE requires enabling [[operations/network-encryption#minio-tls|Network Encryption (TLS)]].

## Secure Erasure and Locking

MinIO requires access to the Encryption Key (EK) and external Key Management
System (KMS) used as part of SSE operations to decrypt an object. You can use
this dependency to securely erase and lock objects from access by disabling
access to the EK or KMS used for encryption.

General strategies include, but are not limited to:

Seal the KMS such that it cannot be accessed by MinIO server anymore. This
locks all SSE-KMS or SSE-S3 encrypted objects protected by any EK stored on
the KMS. The encrypted objects remain unreadable as long as the KMS remains
sealed.

Seal/Unmount an EK. This locks all SSE-KMS or SSE-S3 encrypted objects
protected by that EK. The encrypted objects remain unreadable as long
as the CMK(s) remains sealed.

Delete an EK. This renders all SSE-KMS or SSE-S3 encrypted objects protected
by that EK as permanently unreadable. The combination of deleting an EK and
deleting the data may fulfill regulatory requirements around secure deletion
of data.

Deleting an EK is typically irreversible. Exercise extreme caution
before intentionally deleting a master key.

For more information, see:

[[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms-erasure-locking|SSE-KMS Secure Erasure and Locking]]

[[administration/server-side-encryption/server-side-encryption-sse-s3#minio-encryption-sse-s3-erasure-locking|SSE-S3 Secure Erasure and Locking]]

[[administration/server-side-encryption/server-side-encryption-sse-c#minio-encryption-sse-c-erasure-locking|SSE-C Secure Erasure and Locking]]

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).