---
title: "mc admin policy create — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-policy-create.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-policy-create"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin policy create — SILO (MinIO Object Storage)"
---
# mc admin policy create

## Syntax

Creates a new policy on the target MinIO deployment.

MinIO deployments include the following [[administration/identity-access-management/policy-based-access-control#minio-policy-built-in|built-in policies]] by default:

[[administration/identity-access-management/policy-based-access-control#userpolicy.readonly|`readonly`]]

[[administration/identity-access-management/policy-based-access-control#userpolicy.readwrite|`readwrite`]]

[[administration/identity-access-management/policy-based-access-control#userpolicy.diagnostics|`diagnostics`]]

[[administration/identity-access-management/policy-based-access-control#userpolicy.writeonly|`writeonly`]]

EXAMPLE

Consider the following JSON policy document saved at a file called `/tmp/listmybuckets.json`:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "s3:ListAllMyBuckets"
         ],
         "Resource": [
            "arn:aws:s3:::*"
         ]
      }
   ]
}
```

The following command creates a new policy called `listmybuckets` on the [[glossary#term-alias|alias]] `myminio` using the policy found at the file `/tmp/listmybuckets.json`.

```
mc admin policy create myminio listmybuckets /tmp/listmybuckets.json
```

SYNTAX

The command has the following syntax:

```
mc admin policy create     \
                TARGET     \
                POLICYNAME \
                POLICYPATH
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

The [`mc admin policy create`](#command-mc.admin.policy.create) command accepts the following arguments:

TARGET[](#mc.admin.policy.create.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment on which to add the new policy.

POLICYNAME[](#mc.admin.policy.create.POLICYNAME)

The name of the policy to add.

Specifying the name of an existing policy overwrites that policy on the [`TARGET`](#mc.admin.policy.create.TARGET) MinIO deployment.

POLICYPATH[](#mc.admin.policy.create.POLICYPATH)

The file path of the policy to add.
The file must be a JSON-formatted file with [IAM-compatible syntax](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies.html) and no more than 2048 characters.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

Create a new policy called `writeonly` from the JSON file at `/tmp/writeonly.json` on the deployment at the alias `myminio`.

```
mc admin policy create myminio writeonly /tmp/writeonly.json
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).