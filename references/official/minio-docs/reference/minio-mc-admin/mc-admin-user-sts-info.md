---
title: "mc admin user sts info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-sts-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-sts-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user sts info — SILO (MinIO Object Storage)"
---
# mc admin user sts info

## Syntax

The [`mc admin user sts info`](#command-mc.admin.user.sts.info) command retrieves information on the specified STS credential, such as the parent [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO user]] who generated the credentials, associated policies, and expiration.

STS credentials provide temporary access to the MinIO deployment.

EXAMPLE

The following command retrieves information on the STS credentials with specified access key:

```
mc admin user sts info myminio/ "J123C4ZXEQN8RK6ND35I"
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user sts info          \
                                [--policy]    \
                                ALIAS         \
                                STSACCESSKEY
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.user.sts.info.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

STSACCESSKEY[](#mc.admin.user.sts.info.STSACCESSKEY)
Required

The access key for the STS credentials.

--policy[](#mc.admin.user.sts.info.-policy)
Optional

Prints the policy attached to the specified STS credentials in JSON format.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Behavior

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).