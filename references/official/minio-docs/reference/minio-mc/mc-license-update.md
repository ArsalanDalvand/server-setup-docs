---
title: "mc license update — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-license-update.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-license-update"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc license update — SILO (MinIO Object Storage)"
---
# mc license update

## Description

Use the [`mc license update`](#command-mc.license.update) command to replace a license key for a deployment.

For deployments registered for [MinIO SUBNET](https://min.io/pricing?jmp=docs), MinIO automatically checks for and updates the license every month.

## Examples

### Update the License Key for a Deployment with Alias minio1

```
mc license update minio1 license.key
```

## Syntax

The command has the following syntax:

```
mc [GLOBALFLAGS] license update                   \
                         ALIAS                    \
                         [LICENSE-FILE-WITH-PATH] \
                         [--airgap]
```

### Parameters

ALIAS[](#mc.license.update.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

LICENSE-FILE-WITH-PATH[](#mc.license.update.LICENSE-FILE-WITH-PATH)
Optional

The path (relative to the current working directory) and file name of the key to use to update the deployment’s license.

To download the API key from SUBNET:

Log in to [MinIO SUBNET](https://min.io/pricing?jmp=docs)

Go to the Deployments tab

Select the API Key button near the top of the page on the right side of the account statistics information box

Select copy button to the right of the key field to copy the key value to your clipboard

--airgap[](#mc.license.update.-airgap)
Optional

Use in environments without network access to SUBNET (for example, airgapped, firewalled, or similar configuration).

If the deployment is airgapped, but the local device where you are using the [[reference/minio-mc#minio-client|minio client]] has network access, you do not need to use the `--airgap` flag.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).