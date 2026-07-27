---
title: "Server-Side Encryption with Client-Managed Keys (SSE-C) — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/server-side-encryption/server-side-encryption-sse-c.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/server-side-encryption/server-side-encryption-sse-c"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Server-Side Encryption with Client-Managed Keys (SSE-C) — SILO (MinIO Object Storage)"
---
# Server-Side Encryption with Client-Managed Keys (SSE-C)

MinIO Server-Side Encryption (SSE) protects objects as part of write operations,
allowing clients to take advantage of server processing power to secure objects
at the storage layer (encryption-at-rest). SSE also provides key functionality
to regulatory and compliance requirements around secure locking and erasure.

The procedure on this page configures and enables Server-Side Encryption
with Client-Managed Keys (SSE-C). MinIO SSE-C supports client-driven
encryption of objects before writing the object to the drive. Clients must
specify the correct key to decrypt objects for read operations.

MinIO SSE-C is functionally compatible with Amazon
[Server-Side Encryption with Customer-Provided Keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/ServerSideEncryptionCustomerKeys.html).

## Secure Erasure and Locking

SSE-C protects objects using an EK specified by the client as part
of the write operation. Assuming the client-side key management
supports disabling or deleting these keys:

Disabling the EK temporarily locks any objects encrypted using that

EK by rendering them unreadable. You can later enable the EK to
resume normal read operations on those objects.

Deleting the EK renders all objects encrypted by that EK

permanently unreadable. If the client-side KMS does not support
backups of the EK, this process is irreversible.

The scope of a single EK depends on the number of write operations
which specified that EK when requesting SSE-C encryption.

## Considerations

### SSE-C with Replication

Changed in version Server: RELEASE.2024-03-30T09-41-56Z

Objects encrypted with SSE-C can replicate through both site replication or bucket replication.
Previous versions of MinIO Object Store did not replicate SSE-C encrypted objects.

SSE-C encrypted objects that are compressed are not compatible with MinIO [[administration/bucket-replication#minio-bucket-replication|bucket replication]] or [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]].
Use [[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms|SSE-KMS]] or [[administration/server-side-encryption/server-side-encryption-sse-s3#minio-encryption-sse-s3|SSE-S3]] to ensure encrypted objects are compatible with replication.

### SSE-C Overrides SSE-S3 and SSE-KMS

Encrypting an object using SSE-C prevents MinIO from applying
[[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms|SSE-KMS]] or
[[administration/server-side-encryption/server-side-encryption-sse-s3#minio-encryption-sse-s3|SSE-S3]] encryption to that object.

## Quickstart

MinIO SSE-C requires the client to perform all key creation and storage operations.

This procedure uses [[reference/minio-mc#command-mc|`mc`]] for performing operations on the source MinIO deployment.
Install [[reference/minio-mc#command-mc|`mc`]] on a machine with network access to the source deployment.
See the `mc` [[reference/minio-mc#mc-install|Installation Quickstart]] for instructions on downloading and installing `mc`.

The SSE-C key must be a 256-bit raw encoded string or a hex encoded string.
The client application is responsible for generation and storage of the encryption key.
MinIO does not store SSE-C encryption keys and cannot decrypt SSE-C encrypted objects without the client-managed key.

Note

Support for hex encoded keys was added in MinIO Client `RELEASE.2024-06-20T14-50-54Z`.

### 1) Generate the Encryption Key

Generate the 256-bit base64 raw encoded string or a hex encoded string for use as the encryption key.

The following example generates a string that meets the encryption key requirements.
The resulting string is appropriate for non-production environments:

```
cat /dev/urandom | head -c 32 | base64 -
```

Defer to your organizations requirements for generating cryptographically secure encryption keys.

Copy the encryption key for use in the next step.

### 2) Encrypt an Object using SSE-C

MinIO supports the following AWS S3 headers for specifying SSE-C encryption:

`X-Amz-Server-Side-Encryption-Customer-Algorithm` set to `AES256`.

`X-Amz-Server-Side-Encryption-Customer-Key` set to the encryption key value.

`X-Amz-Server-Side-Encryption-Customer-Key-MD5` to the 128-bit MD5 digest of the encryption key.

The MinIO [[reference/minio-mc#command-mc|`mc`]] commandline tool S3-compatible SDKs include specific syntax
for setting headers. Certain [[reference/minio-mc#command-mc|`mc`]] commands like [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] include specific
arguments for enabling SSE-S3 encryption:

```
mc cp ~/data/mydata.json ALIAS/BUCKET/mydata.json \
   --encrypt-key "ALIAS/BUCKET/=c2VjcmV0ZW5jcnlwdGlvbmtleWNoYW5nZW1lMTIzNAo="
```

Replace [[reference/minio-mc/mc-encrypt-set#mc.encrypt.set.ALIAS|`ALIAS`]] with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment on which you want to write
the SSE-C encrypted object.

Replace [[reference/minio-mc/mc-encrypt-set#mc.encrypt.set.ALIAS|`BUCKET`]]  with the full path to the
bucket or bucket prefix to which you want to write the SSE-C encrypted object.

### 3) Copy an SSE-C Encrypted Object

MinIO supports the following AWS S3 headers for copying an SSE-C encrypted
object to another S3-compatible service:

`X-Amz-Copy-Source-Server-Side-Encryption-Algorithm` set to `AES256`

`X-Amz-Copy-Source-Server-Side-Encryption-Key` set to the encryption key
value. The copy operation will fail if the specified key does not match
the key used to SSE-C encrypt the object.

`X-Amz-Copy-Source-Server-Side-Encryption-Key-MD5` set to the 128-bit MD5
digest of the encryption key.

The MinIO [[reference/minio-mc#command-mc|`mc`]] commandline tool S3-compatible SDKs include specific syntax
for setting headers. Certain [[reference/minio-mc#command-mc|`mc`]] commands like [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] include specific
arguments for enabling SSE-S3 encryption:

```
mc cp SOURCE/BUCKET/mydata.json TARGET/BUCKET/mydata.json  \
--encrypt-key "SOURCE/BUCKET/=c2VjcmV0ZW5jcnlwdGlvbmtleWNoYW5nZW1lMTIzNAo=,TARGET/BUCKET/=c2VjcmV0ZW5jcnlwdGlvbmtleWNoYW5nZW1lMTIzNAo="
```

Replace [[reference/minio-mc/mc-encrypt-set#mc.encrypt.set.ALIAS|`SOURCE/BUCKET`]] with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment from which you are reading the
encrypted object and the full path to the
bucket or bucket prefix from which you want to read the SSE-C encrypted
object.

Replace [[reference/minio-mc/mc-encrypt-set#mc.encrypt.set.ALIAS|`TARGET/BUCKET`]] with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment from which you are writing the
encrypted object and the full path to the
bucket or bucket prefix to which you want to write the SSE-C encrypted
object.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).