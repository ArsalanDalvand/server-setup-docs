---
title: "mc ilm tier ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm-tier-ls.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm-tier-ls"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm tier ls — SILO (MinIO Object Storage)"
---
# mc ilm tier ls

Changed in version RELEASE.2022-12-24T15-21-38Z: [`mc ilm tier ls`](#command-mc.ilm.tier.ls) replaces `mc admin tier ls`.

## Description

The [`mc ilm tier ls`](#command-mc.ilm.tier.ls) command shows the remote tiers configured on a deployment.

The [`mc ilm tier list`](#command-mc.ilm.tier.list) command has equivalent functionality to [`mc ilm tier ls`](#command-mc.ilm.tier.ls).

## Syntax

The command has the following syntax:

EXAMPLE

The following example outputs a list of the existing remote tiers on the `myminio` deployment.

```
mc ilm tier ls myminio
```

SYNTAX

The command has the following syntax:

```
mc ilm tier ls TARGET TIER_NAME
```

### Parameters

The command accepts the following argument:

TARGET[](#mc.ilm.tier.ls.TARGET)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment on which the desired tier exists.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

## Required Permissions

For permissions required for reviewing a tier, refer to the [[reference/minio-mc/mc-ilm-tier#minio-mc-ilm-tier-permissions|required permissions]] on the parent command.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).