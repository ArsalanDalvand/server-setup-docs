---
title: "mc admin policy info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-policy-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-policy-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin policy info — SILO (MinIO Object Storage)"
---
# mc admin policy info

## Syntax

Returns the specified policy in JSON format if it exists on the target MinIO deployment.

EXAMPLE

The following command displays the contents of the `writeonly` policy on the [[glossary#term-alias|alias]] `myminio`.

```
mc admin policy info myminio writeonly
```

SYNTAX

The command has the following syntax:

```
mc admin policy info TARGET POLICYNAME
                     [--policy-file, -f <path>]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

The [`mc admin policy info`](#command-mc.admin.policy.info) command accepts the following arguments:

TARGET[](#mc.admin.policy.info.TARGET)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment from which to display the specified policy.

POLICYNAME[](#mc.admin.policy.info.POLICYNAME)
Required

The name of the policy whose details you want to display.

--policy-file[](#mc.admin.policy.info.-policy-file)
Optional

Specifly the path of a file to write the contents of the specified policy JSON.
If the path already exists, the command overwrites the existing file with the contents of the specified file.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

Display the contents of the `writeonly` policy on the [[glossary#term-alias|alias]] `myminio`.

```
mc admin policy info myminio writeonly
```

Show information on a given policy and write the policy JSON content to /tmp/policy.json.

```
mc admin policy info myminio writeonly --policy-file /tmp/policy.json
```

### Output

The command returns output that resembles the following:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "s3:PutObject"
         ],
         "Resource": [
            "arn:aws:s3:::*"
         ]
      }
   ]
}
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).