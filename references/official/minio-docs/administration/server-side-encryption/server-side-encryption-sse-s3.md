---
title: "Server-Side Encryption Per-Deployment Key (SSE-S3) — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/server-side-encryption/server-side-encryption-sse-s3.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/server-side-encryption/server-side-encryption-sse-s3"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Server-Side Encryption Per-Deployment Key (SSE-S3) — SILO (MinIO Object Storage)"
---
# Server-Side Encryption Per-Deployment Key (SSE-S3)

MinIO Server-Side Encryption (SSE) protects objects as part of write operations,
allowing clients to take advantage of server processing power to secure objects
at the storage layer (encryption-at-rest). SSE also provides key functionality
to regulatory and compliance requirements around secure locking and erasure.

MinIO SSE uses the [MinIO Key Encryption Service (KES)](https://docs.min.io/community/minio-kes/) and an
external Key Management Service (KMS) for performing secured cryptographic
operations at scale. MinIO also supports client-managed key management, where
the application takes full responsibility for creating and managing encryption
keys for use with MinIO SSE.

MinIO SSE-S3 en/decrypts objects using an External Key (EK) managed by a
Key Management System (KMS). You must specify the EK using the
[[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]] environment variable when starting up the
MinIO server. MinIO uses the same EK for all SSE-S3 cryptographic operations.

You can enable bucket-default SSE-S3 encryption using the [[reference/minio-mc/mc-encrypt-set#command-mc.encrypt.set|`mc encrypt set`]] command:

```
mc encrypt set sse-s3 play/mybucket
```

Replace `play/mybucket` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] and bucket
on which you want to enable automatic SSE-KMS encryption.

MinIO SSE-S3 is functionally compatible with AWS S3 [Server-Side Encryption with Amazon S3-Managed Keys](https://docs.aws.amazon.com/AmazonS3/latest/userguide/UsingServerSideEncryption.html) while expanding support to include the following KMS providers:

[AWS Secrets Manager](https://docs.min.io/community/minio-kes/integrations/aws-secrets-manager/)

[Azure KeyVault](https://docs.min.io/community/minio-kes/integrations/azure-keyvault/)

[Entrust KeyControl](https://docs.min.io/community/minio-kes/integrations/entrust-keycontrol/)

[Fortanix SDKMS](https://docs.min.io/community/minio-kes/integrations/fortanix-sdkms/)

[Google Cloud Secret Manager](https://docs.min.io/community/minio-kes/integrations/google-cloud-secret-manager/)

[HashiCorp Vault](https://docs.min.io/community/minio-kes/integrations/hashicorp-vault-keystore/)

[Thales CipherTrust Manager (formerly Gemalto KeySecure)](https://docs.min.io/community/minio-kes/integrations/thales-ciphertrust/)

## Quickstart

Important

Enabling SSE on a MinIO deployment automatically encrypts the backend data for that deployment using the default encryption key.

MinIO requires access to KES and the external KMS to decrypt the backend and start normally.
The KMS must maintain and provide access to the [[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]].
You cannot disable KES later or “undo” the SSE configuration at a later point.

The following procedure uses the `play` MinIO KES sandbox for
supporting SSE with SSE-S3 in evaluation and early development environments.

For extended development or production environments, use one of the following
supported external Key Management Services (KMS):

[AWS Secrets Manager](https://docs.min.io/community/minio-kes/integrations/aws-secrets-manager/)

[Azure KeyVault](https://docs.min.io/community/minio-kes/integrations/azure-keyvault/)

[Entrust KeyControl](https://docs.min.io/community/minio-kes/integrations/entrust-keycontrol/)

[Fortanix SDKMS](https://docs.min.io/community/minio-kes/integrations/fortanix-sdkms/)

[Google Cloud Secret Manager](https://docs.min.io/community/minio-kes/integrations/google-cloud-secret-manager/)

[HashiCorp Vault](https://docs.min.io/community/minio-kes/integrations/hashicorp-vault-keystore/)

[Thales CipherTrust Manager (formerly Gemalto KeySecure)](https://docs.min.io/community/minio-kes/integrations/thales-ciphertrust/)

Important

The MinIO KES `Play` sandbox is public and grants root access to all created External Keys (EK).
Any EK stored on the `Play` sandbox may be accessed or destroyed at any time, rendering protected data vulnerable or permanently unreadable.

Never use the `Play` sandbox to protect data you cannot afford to lose or reveal.

Never generate EK using names that reveal private, confidential, or internal naming conventions for your organization.

Never use the `Play` sandbox for production environments.

This procedure requires the following components:

Install [[reference/minio-mc#command-mc|`mc`]] on a machine with network access to the source deployment.
See the `mc` [[reference/minio-mc#mc-install|Installation Quickstart]] for instructions on downloading and installing `mc`.

Install [MinIO Key Encryption Service (KES)](https://docs.min.io/community/minio-kes/) on a machine with internet access.
See the KES [Getting Started](https://docs.min.io/community/minio-kes/tutorials/getting-started/) guide for instructions on downloading, installing, and configuring KES.

### 1) Create an Encryption Key for SSE-S3 Encryption

Use the [kes](https://docs.min.io/community/minio-kes/cli) command line tool to create a new External Key (EK) for use with SSE-S3 Encryption.

The following command retrieves the root [identity](https://github.com/minio/kes/wiki/Configuration#policy-configuration) for the KES server connected to the KES `play` sandbox:

```
curl -sSL --tlsv1.2 \
  -O 'https://raw.githubusercontent.com/minio/kes/master/root.key' \
  -O 'https://raw.githubusercontent.com/minio/kes/master/root.cert'
```

Set the following environment variables in the terminal or shell:

```
export KES_CLIENT_KEY=root.key
export KES_CLIENT_CERT=root.cert
```

`KES_CLIENT_KEY`

The private key for an [identity](https://docs.min.io/community/minio-kes/concepts/#authorization) on the KES server.
The identity must grant access to at minimum the `/v1/create`, `/v1/generate`, and `/v1/list` [API endpoints](https://docs.min.io/community/minio-kes/concepts/server-api/).
This step uses the `root` identity for the MinIO `play` KES sandbox, which provides access to all operations on the KES server.

`KES_CLIENT_CERT`

The corresponding certificate for the [identity](https://docs.min.io/community/minio-kes/concepts/#authorization) on the KES server.
This step uses the `root` identity for the MinIO `play` KES sandbox, which provides access to all operations on the KES server.

The following command creates a new EK through the [KES CLI](https://docs.min.io/community/minio-kes/cli/kes-key/create/):

```
kes key create my-minio-sse-s3-key
```

This tutorial uses the example `my-minio-sse-s3-key` name for ease of reference.
Specify a unique key name to prevent collision with existing keys.

### 2) Configure MinIO for SSE-S3 Object Encryption

Specify the following environment variables in the shell or terminal on each
MinIO server host in the deployment:

```
export MINIO_KMS_KES_ENDPOINT=https://play.min.io:7373
export MINIO_KMS_KES_API_KEY=<API-key-identity-string-from-KES> # Replace with the key string for your credentials
export MINIO_KMS_KES_KEY_NAME=my-minio-sse-s3-key
```

Note

An API key is the preferred way to authenticate with the KES server, as it provides a streamlined and secure authentication process to the KES server.

Alternatively, specify the [[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_FILE|`MINIO_KMS_KES_KEY_FILE`]] and [[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_CERT_FILE|`MINIO_KMS_KES_CERT_FILE`]] instead of [[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_API_KEY|`MINIO_KMS_KES_API_KEY`]].

API keys are mutually exclusive with certificate-based authentication.
Specify either the API key variable or the Key File and Cert File variables.

The documentation on this site uses API keys.

[[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_ENDPOINT|`MINIO_KMS_KES_ENDPOINT`]]

The endpoint for the MinIO `Play` KES service.

[[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_FILE|`MINIO_KMS_KES_KEY_FILE`]]

The private key file corresponding to an
[identity](https://github.com/minio/kes/wiki/Configuration#policy-configuration)
on the KES service. The identity must grant permission to
create, generate, and decrypt keys. Specify the same
identity key file as the `KES_KEY_FILE` environment variable
in the previous step.

[[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_CERT_FILE|`MINIO_KMS_KES_CERT_FILE`]]

The public certificate file corresponding to an
[identity](https://github.com/minio/kes/wiki/Configuration#policy-configuration)
on the KES service. The identity must grant permission to
create, generate, and decrypt keys. Specify the same
identity certificate as the `KES_CERT_FILE` environment
variable in the previous step.

[[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]]

The name of the External Key (EK) to use for
performing SSE encryption operations. KES retrieves the EK from
the configured Key Management System (KMS). Specify the name of the
key created in the previous step.

### 3) Restart the MinIO Deployment to Enable SSE-S3

You must restart the MinIO deployment to apply the configuration changes.
Use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment.

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to restart.

### 4) Configure Automatic Bucket Encryption

Optional

You can skip this step if you intend to use only client-driven SSE-S3.

Use the [[reference/minio-mc/mc-encrypt-set#command-mc.encrypt.set|`mc encrypt set`]] command to enable automatic SSE-S3 protection
of all objects written to a specific bucket.

```
mc encrypt set sse-s3 ALIAS/BUCKET
```

Replace [[reference/minio-mc/mc-encrypt-set#mc.encrypt.set.ALIAS|`ALIAS`]] with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment on which you enabled SSE-S3.

Replace [[reference/minio-mc/mc-encrypt-set#mc.encrypt.set.ALIAS|`BUCKET`]]  with the full path to the
bucket or bucket prefix on which you want to enable automatic SSE-S3.

## Secure Erasure and Locking

SSE-S3 protects objects using an EK specified at server startup
using the [[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]] environment variable. MinIO
therefore requires access to that EK for decrypting that object.

Disabling the EK temporarily locks SSE-S3-encrypted objects in the
deployment by rendering them unreadable. You can later enable the EK
to resume normal read operations.

Deleting the EK renders all SSE-S3-encrypted objects in the deployment
permanently unreadable. If the KMS does not have or support backups
of the EK, this process is irreversible.

The scope of the EK depends on:

Which buckets specified automatic SSE-S3 encryption, and

Which write operations requested SSE-S3 encryption.

## Encryption Process

Note

The following section describes MinIO internal logic and functionality.
This information is purely educational and is not necessary for
configuring or implementing any MinIO feature.

SSE-S3 uses an External Key (EK) managed by the configured Key Management
System (KMS) for performing cryptographic operations and protecting objects.
The table below describes each stage of the encryption process:

Stage

Description

SSE-Enabled Write Operation

MinIO receives a write operation requesting SSE-S3 encryption.
MinIO uses the key name specified to
[[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]] as the External Key (EK).

Generate the Data Encryption Key (DEK)

MinIO generates a Data Encryption Key (DEK) using the EK.
Specifically, [MinIO Key Encryption Service (KES)](https://github.com/minio/kes) requests a new cryptographic key from the KMS using the EK as the “root” key.

KES returns both the plain-text and an EK-encrypted representation of the DEK.
MinIO stores the encrypted representation as part of the object metadata.

Generate the Key Encryption Key (KEK)

MinIO uses a deterministic algorithm to generate a 256-bit unique Key Encryption Key (KEK).
The key-derivation algorithm uses a pseudo-random function that takes the plain-text DEK, a randomly generated initialization vector, and a context consisting of values like the bucket and object name.

MinIO generates the KEK at the time of each cryptographic encryption or decryption operation and never stores the KEK to a drive.

Generate the Object Encryption Key (OEK)

MinIO generates a random 256-bit unique Object Encryption Key (OEK) and uses that key to encrypt the object.
MinIO never stores the plaintext representation of the OEK on a drive.
The plaintext OEK resides in RAM during cryptographic operations.

Encrypt the Object

MinIO uses the OEK to encrypt the object prior to storing the
object to a drive. MinIO then encrypts the OEK with the KEK.

MinIO stores the encrypted representation of the OEK and DEK as part
of the metadata.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).