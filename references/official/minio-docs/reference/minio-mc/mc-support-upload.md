---
title: "mc support upload — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-upload.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-upload"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support upload — SILO (MinIO Object Storage)"
---
# mc support upload

## Description

[`mc support upload`](#command-mc.support.upload) copies a file from the local file system to a SUBNET ticket.

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Syntax

The [[reference/minio-mc/mc-support-profile#command-mc.support.profile|`mc support profile`]] command has the following syntax:

```
mc [GLOBALFLAGS] support profile              \
                         ALIAS                \
                         FILE                 \
                         [--comment "string"] \
                         [--enc]              \
                         [--issue integer]
```

### Parameters

ALIAS[](#mc.support.upload.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

FILE[](#mc.support.upload.FILE)
Required

The path to the file to upload to SUBNET.

--comment[](#mc.support.upload.-comment)
Optional

Include a message to the issue when uploading the file.

--enc[](#mc.support.upload.-enc)
Optional

Encrypt contents of the upload.
The key used for the encryption is only accessible to MinIO.

--issue[](#mc.support.upload.-issue)
Optional

Specify the issue number to which to add the file.
If not specified, the file uploads to the generic issue number `0`.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Upload a file to an issue

This command uploads the file `./trace.log` from the local file system to the SUBNET issue number 10001 for the deployment with alias `minio1`.

```
mc support upload --issue 10001 minio1 ./trace.log
```

### Upload a file to an issue with a comment for MinIO Engineers

This command uploads the file `./trace.log` from the local file system to the SUBNET issue number 10001 for the deployment with alias `minio1`.
The command also includes a comment available to MinIO Engineers about the file.

```
mc support upload --issue 10001 --comment "here is the requested trace log" minio1 ./trace.log
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).