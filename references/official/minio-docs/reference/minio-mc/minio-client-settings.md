---
title: "MinIO Client Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/minio-client-settings.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/minio-client-settings"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Client Settings — SILO (MinIO Object Storage)"
---
# MinIO Client Settings

This page covers settings for the [[reference/minio-mc#minio-client|MinIO Client]].

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

## Settings

### Host Credentials

Use this setting to add a temporary alias to use for mc commands.
For example, for use with scripting.

The temporary alias uses the [AWS s3v4 signature](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html).

Environment Variable

MC_HOST_<ALIAS>[](#envvar.MC_HOST_-ALIAS)

Replace `<ALIAS>` at the end of the environment variable with the `alias` to set the host for.

Configuration Setting

This setting does not have a configuration setting option.

Use [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] to configure an [[reference/minio-mc/mc-alias-set#alias|alias]].

#### Examples

Static Credentials

Syntax

```
export MC_HOST_<alias>=https://<Access Key>:<Secret Key>@<YOUR-S3-ENDPOINT>
```

Example

```
export MC_HOST_myalias=https://Q3AM3UQ867SPQQA43P2F:[email protected]
```

Security Token Service (STS) Credentials

Syntax

```
export MC_HOST_<alias>=https://<Access Key>:<Secret Key>:<Session Token>@<YOUR-S3-ENDPOINT>
```

Example

```
export MC_HOST_myalias=https://Q3AM3UQ867SPQQA43P2F:zuf+tfteSlswRu7BJ86wekitnifILbZam1KYY3TG:eyJhbGciOiJIUzUxMiIsInR5cCI6IkpXVCJ9.eyJhY2Nlc3NLZXkiOiJOVUlCT1JaWVRWMkhHMkJNUlNYUiIsImF1ZCI6IlBvRWdYUDZ1Vk80NUlzRU5SbmdEWGo1QXU1WWEiLCJhenAiOiJQb0VnWFA2dVZPNDVJc0VOUm5nRFhqNUF1NVlhIiwiZXhwIjoxNTM0ODk2NjI5LCJpYXQiOjE1MzQ4OTMwMjksImlzcyI6Imh0dHBzOi8vbG9jYWxob3N0Ojk0NDMvb2F1dGgyL3Rva2VuIiwianRpIjoiNjY2OTZjZTctN2U1Ny00ZjU5LWI0MWQtM2E1YTMzZGZiNjA4In0.eJONnVaSVHypiXKEARSMnSKgr-2mlC2Sr4fEGJitLcJF_at3LeNdTHv0_oHsv6ZZA3zueVGgFlVXMlREgr9LXA@play.min.io
```

### STS Service

New in version mc: RELEASE.2023-11-06T04-19-23Z

Use this setting to add an STS endpoint to use for mc commands.

Changed in version mc: RELEASE.2023-12-02T02-03-28Z

Supports adding multiple environment variables by alias.

Environment Variable

MC_STS_ENDPOINT_<alias>[](#envvar.MC_STS_ENDPOINT_-alias)

```
export MC_STS_ENDPOINT_myalias=https://sts.minio-operator.svc.cluster.local:4223/sts/ns-1
```

Configuration Setting

This setting does not have a configuration setting option.

### Web Token Identity

New in version mc: RELEASE.2023-11-06T04-19-23Z

Use this setting to add a web token identity to use for mc commands.

Changed in version mc: RELEASE.2023-12-02T02-03-28Z

Supports adding multiple environment variables by alias.

Environment Variable

MC_WEB_IDENTITY_TOKEN_<alias>[](#envvar.MC_WEB_IDENTITY_TOKEN_-alias)

```
export MC_WEB_IDENTITY_TOKEN_FILE_myalias=/var/run/secrets/kubernetes.io/serviceaccount/token
```

Configuration Setting

This setting does not have a configuration setting option.

### Configuration Directory

Specify the path to the configuration folder the MinIO Client should use.

Environment Variable

MC_CONFIG_DIR[](#envvar.MC_CONFIG_DIR)

Configuration Setting

This setting does not have a configuration setting option.

### Progress Bar

Disable the MinIO Client progress bar.

Environment Variable

MC_QUIET[](#envvar.MC_QUIET)

Configuration Setting

This setting does not have a configuration setting option.

### Pager

New in version mc: RELEASE.2024-04-29T09-56-05Z

Disable the pager functionality of the MinIO Client in the CLI.
When used, output prints to raw `STDOUT` instead.

Environment Variable

MC_DISABLE_PAGER[](#envvar.MC_DISABLE_PAGER)

Configuration Setting

This setting does not have a configuration setting option.

### Color Theme

Disable the color theme used for MinIO Client output.

Environment Variable

MC_NO_COLOR[](#envvar.MC_NO_COLOR)

Configuration Setting

This setting does not have a configuration setting option.

### JSON

Enable formatting the output as JSON lines.

Environment Variable

MC_JSON[](#envvar.MC_JSON)

Configuration Setting

This setting does not have a configuration setting option.

### Debug

Enable the debug output.

Environment Variable

MC_DEBUG[](#envvar.MC_DEBUG)

Configuration Setting

This setting does not have a configuration setting option.

### Disable SSL

Disable SSL certificate verification.

Environment Variable

MC_INSECURE[](#envvar.MC_INSECURE)

Configuration Setting

This setting does not have a configuration setting option.

### Limit Download Bandwidth

Limit the download bandwidth the MinIO Client uses for certain commands.

Environment Variable

MC_LIMIT_DOWNLOAD[](#envvar.MC_LIMIT_DOWNLOAD)

Configuration Setting

This setting does not have a configuration setting option.

If not specified, the MinIO Client uses all available bandwidth.

Limit client-side download rates to no more than the specified rate in KiB/s, MiB/s, or GiB/s. This affects only the download from the local device running the MinIO Client. Valid units include:

B for bytes

K for kilobytes

M for megabytes

G for gigabytes

Ki for kibibytes

Mi for mibibytes

Gi for gibibytes

For example, to limit download rates to no more than 1 GiB/s, use the following on a Linux system:

```
export MC_LIMIT_DOWNLOAD=1G
```

Refer to your operating system instructions for equivalent commands on non-Linux systems.

### Limit Upload Bandwidth

Limit the upload bandwidth the MinIO Client uses for certain commands.

Environment Variable

MC_LIMIT_UPLOAD[](#envvar.MC_LIMIT_UPLOAD)

Configuration Setting

This setting does not have a configuration setting option.

If not specified, the MinIO Client uses all available bandwidth.

Limit client-side upload rates to no more than the specified rate in KiB/s, MiB/s, or GiB/s. This affects only the upload from the local device running the MinIO Client. Valid units include:

B for bytes

K for kilobytes

M for megabytes

G for gigabytes

Ki for kibibytes

Mi for mibibytes

Gi for gibibytes

For example, to limit upload rates to no more than 1 GiB/s, use the following on a Linux system:

```
export MC_LIMIT_UPLOAD=1G
```

Refer to your operating system instructions for equivalent commands on non-Linux systems.

### SSE-KMS Encryption

Encrypt and decrypt options using [[operations/server-side-encryption#minio-sse-data-encryption|SSE-KMS]] with server managed keys.

Environment Variable

MC_ENC_KMS[](#envvar.MC_ENC_KMS)

Specify the key with the [`MC_ENC_KMS`](#envvar.MC_ENC_KMS) environment variable.

Configuration Setting

This setting does not have a configuration setting option.

### SSE-S3 Encryption

Encrypt and decrypt options using [[operations/server-side-encryption#minio-sse-data-encryption|SSE-KMS]] with server managed keys.

Environment Variable

MC_ENC_S3[](#envvar.MC_ENC_S3)

Specify the key to use for performing SSE-S3 encryption.
The specified value must match the encryption key set in [[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]].

Configuration Setting

This setting does not have a configuration setting option.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).