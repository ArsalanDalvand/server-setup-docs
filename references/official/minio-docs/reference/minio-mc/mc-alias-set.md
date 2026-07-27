---
title: "mc alias set — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-alias-set.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-alias-set"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc alias set — SILO (MinIO Object Storage)"
---
# mc alias set

## Syntax

The [`mc alias set`](#command-mc.alias.set) command adds or updates an alias to the local
mc configuration.

EXAMPLE

The following command adds an [alias](#alias) for a MinIO
deployment `myminio` running at the URL
`https://myminio.example.net`. mc uses the specified
username and password for authenticating to the MinIO deployment:

```
mc alias set myminio https://myminio.example.net minioadminuser minioadminpassword
```

If the `myminio` alias already exists, the command overwrites that
alias with the new URL, access key, and secret key.

SYNTAX

The [`mc alias set`](#command-mc.alias.set) command has the following syntax:

```
mc [GLOBALFLAGS] alias set \
                 [--api "string"]                           \
                 [--path "string"]                          \
                 ALIAS                                      \
                 URL                                        \
                 ACCESSKEY                                  \
                 SECRETKEY
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.alias.set.ALIAS)

Required The name to associate to the S3-compatible service.
Aliases are case-sensitive and must meet the following requirements:

Contain only [ASCII](https://en.wikipedia.org/wiki/ASCII) lower case letters (`a-z`), upper case letters (`A-Z`), numbers (`[0-9]`), hyphen (`-`), or underscore (`_`).

2 or more characters in length.

The first character must be a letter.

Changed in version RELEASE.2024-01-11T05-49-32Z: An alias may also be a single letter (`a-z` or `A-Z`).

Examples of some valid alias values include:

`myminio`

`Test-1`

`A`

`a`

URL[](#mc.alias.set.URL)

Required The URL to the S3-compatible service endpoint. For example:

`https://minio.example.net`

ACCESSKEY[](#mc.alias.set.ACCESSKEY)

Required

The access key for authenticating to the S3 service.

SECRETKEY[](#mc.alias.set.SECRETKEY)

Required

The secret key for authenticating to the S3 service.

--api[](#mc.alias.set.-api)

Optional

Specifies the signature calculation method to use when connecting to the
S3-compatible service. Supports the following values:

`S3v4` (Default)

`S3v2`

Note

AWS Signature V2 is considered
[deprecated](https://aws.amazon.com/blogs/aws/amazon-s3-update-sigv2-deprecation-period-extended-modified/)
by AWS. [`mc alias set`](#command-mc.alias.set) includes this option only for S3 buckets
or services still reliant on the Signature V2.

Use `S3v4` unless explicitly required by the S3-compatible service.
MinIO server does not rely on nor require `S3v2`, nor are all API
operations available on `S3v2`.

--path[](#mc.alias.set.-path)

Optional

Specifies the bucket path lookup setting used by the server. Supports the
following values:

`"auto"` (Default)

`"on"`

`"off"`

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Add or Update an Alias for a MinIO Deployment

Use [`mc alias set`](#command-mc.alias.set) to add an S3-compatible service for use with
mc:

Example

The following command creates a new alias `myminio` pointing at a
MinIO deployment at `https://minio.example.net`. The
alias uses the `miniouser` and `miniopassword` credentials for
performing operations against the deployment.

```
mc alias set myminio https://minio.example.net miniouser miniopassword
```

If the `myminio` alias already exists, the
[`mc alias set`](#command-mc.alias.set) command overwrites that alias with the specified
arguments.

Syntax

```
mc alias set ALIAS HOSTNAME ACCESSKEY SECRETKEY
```

Replace `ALIAS` with the the name to associate to the
MinIO service.

Replace `HOSTNAME` with the URL for any node in the MinIO
deployment. You can alternatively specify the URL for a load balancer
or reverse proxy managing connections to the MinIO deployment.

Replace `ACCESSKEY` and `SECRETKEY` with credentials for a user
on the MinIO deployment.

## Behavior

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

### Required Credentials and Access Control

[`mc alias set`](#command-mc.alias.set) requires specifying an access key and corresponding
secret key for the S3-compatible host. mc functionality is limited
based on the policies associated to the specified credentials. For example, if
the specified credentials do not have read/write access to a specific bucket,
mc cannot perform read or write operations on that bucket.

For more information on MinIO Access Control, see
[[administration/identity-access-management#minio-access-management|Access Management]].

For more complete documentation on S3 Access Control, see
[Amazon S3 Security](https://docs.aws.amazon.com/AmazonS3/latest/userguide/security.html).

For all other S3-compatible services, defer to the documentation for that
service.

### Certificates

The MinIO Client fetches the peer certificate, computes the public key fingerprint, and asks the user whether to accept the deployment’s certificate.

If trusted, the MinIO Client automatically adds the certificate authority to:

`~/.mc/certs/CAs/` on Linux and other Unix-like systems.

`C:\Users\[username]\mc\certs\CAs\` on Windows systems.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).