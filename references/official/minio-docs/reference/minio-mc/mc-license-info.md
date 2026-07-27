---
title: "mc license info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-license-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-license-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc license info — SILO (MinIO Object Storage)"
---
# mc license info

## Description

The [`mc license info`](#command-mc.license.info) command displays information about the MinIO deployment’s license status.
Specifically, whether the deployment uses the AGPLv3 Open Source license of the [MinIO Commercial License](https://min.io/product/subnet?ref=docs).

You must register your deployment with MinIO [MinIO SUBNET](https://min.io/pricing?jmp=docs) to activate your commercial license.

For example, the command returns the following information for an unregistered deployment:

```
You are using GNU AFFERO GENERAL PUBLIC LICENSE Version 3 (https://www.gnu.org/licenses/agpl-3.0.txt)

If you are building proprietary applications, you may want to choose the commercial license
included as part of the Standard and Enterprise subscription plans. (https://min.io/signup?ref=mc)

Applications must otherwise comply with all the GNU AGPLv3 License & Trademark obligations.
```

Use [[reference/minio-mc/mc-license-register#command-mc.license.register|`mc license register`]] to associate your deployment with your SUBNET account.
If you are not already signed up for SUBNET, see the [Registration](https://min.io/pricing?ref=docs) page.

## Examples

### Display the Current License for a Deployment with Alias minio1

```
mc license info minio1
```

If a deployment uses an expired MinIO Commercial License, the command outputs an error message.

## Syntax

The command has the following syntax:

```
mc [GLOBALFLAGS] license info       \
                         ALIAS      \
                         [--airgap]
```

### Parameters

ALIAS[](#mc.license.info.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

--airgap[](#mc.license.info.-airgap)
Optional

Use in environments where the client machine running the [[reference/minio-mc#minio-client|minio client]] does not have network access to SUBNET (for example, airgapped, firewalled, or similar configuration) to display instructions for how to register the deployment with SUBNET.

If the deployment is airgapped, but the local device has network access, you do not need to use the `--airgap` flag.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).