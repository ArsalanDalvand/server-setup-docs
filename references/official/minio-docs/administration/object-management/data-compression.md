---
title: "Data Compression — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/object-management/data-compression.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/object-management/data-compression"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Data Compression — SILO (MinIO Object Storage)"
---
# Data Compression

## Overview

MinIO Server supports compressing objects to reduce disk usage.
Objects are compressed on PUT before writing to disk, and uncompressed on GET before they are sent to the client. This makes the compression process transparent to client applications and services.

Depending on the type of data, compression may also increase overall throughput.
Write throughput for a production deployment is generally 500MB per second or greater per available CPU core in the system.
Decompression is approximately 1 GB per second or greater for each CPU core.

For best results, review MinIO’s [[operations/checklists/hardware#deploy-minio-distributed-recommendations|recommended hardware configuration]] or use [MinIO SUBNET](https://min.io/pricing?jmp=docs) to work directly with engineers for analyzing compression performance.

### Default File Types

Data compression is a global option, the configured settings apply to all buckets in a deployment.
Enabling data compression compresses the following types of data by default:

File Extensions

Media (MIME) Types

`.txt`

`.log`

`.csv`

`.json`

`.tar`

`.xml`

`.bin`

`text/*`

`application/json`

`application/xml`

`binary/octet-stream`

You can control which objects are compressed by specifying the desired file extensions and [media (MIME) types](https://en.wikipedia.org/wiki/Media_type).

Existing objects are not modified

Enabling, disabling, or updating a deployment’s compression settings does not modify existing objects.
New objects are compressed according to the settings in effect at the time they are created.

### Excluded File Types

Some data cannot be effectively compressed.
For example: video, already compressed data, or files less than 4KiB.
MinIO does not compress common incompressible file types, even if they are specified in the compression configuration.

Objects of these types are never compressed:

Object Type

File Extension

Media (MIME) Type

Audio

`audio/*`

Video

`*.mp4`
`*.mkv`
`*.mov`

`video/*`

Image

`*.jpg`
`*.png`
`*.gif`

`application/x-compress` (LZW)

7ZIP Compressed

`*.7z`

BZIP2 Compressed

`*.bz2`

`application/x-bz2`

GZIP Compressed

`*.gz`

`application/x-gzip`

RAR Compressed

`*.rar`

LZMA Compressed

`*.xz`

`application/x-xz`

ZIP Compressed

`*.zip`

`application/zip`
`application-x-zip-compressed`

Smaller than 4 KiB

### Data Compression and Encryption

MinIO supports encrypting compressed objects but recommends against combining compression and encryption without a prior risk assessment.
Before enabling encryption for compressed objects, carefully consider the security needs of your environment.

See [Transparent Data Compression on MinIO](https://blog.min.io/transparent-data-compression/) for more about combining compression and encryption.
[MinIO SUBNET](https://min.io/pricing?jmp=docs) users can [log in](https://subnet.min.io/?ref=docs) and engage with our engineering and security teams to review encryption options.

## Tutorials

### Enable Data Compression

To enable data compression, use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set the [[reference/minio-server/settings/core#mc-conf.compression|`compression`]] key [[reference/minio-server/settings/core#mc-conf.compression.enable|`enable`]] option to `on`.

The following enables compression for new objects of the [default types](#minio-data-compression-default-types):

```
mc admin config set ALIAS compression enable=on
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

Existing uncompressed objects are not modified.
To configure which extensions and types to compress, see [Configure Which Objects to Compress](#minio-data-compression-configure-objects).

To view the current compression settings:

```
mc admin config get ALIAS compression
```

### Disable Data Compression

To disable data compression, use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set the [[reference/minio-server/settings/core#mc-conf.compression|`compression`]] key [[reference/minio-server/settings/core#mc-conf.compression.enable|`enable`]] option to `off`:

The following disables data compression for new objects:

```
mc admin config set ALIAS compression enable=off
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

Existing compressed objects are not modified.

### Configure Which Objects to Compress

Configure the objects to compress by specifying the desired file extensions and media types in [[reference/minio-server/settings/core#mc-conf.compression.extensions|`extensions`]] or [[reference/minio-server/settings/core#mc-conf.compression.mime_types|`mime_types`]] arguments.

The default data compression configuration compresses the following types of data:

File Extensions

Media (MIME) Types

`.txt`

`.log`

`.csv`

`.json`

`.tar`

`.xml`

`.bin`

`text/*`

`application/json`

`application/xml`

`binary/octet-stream`

Default excluded extensions and types are never compressed

Some objects cannot be efficiently compressed.
MinIO will not attempt to compress these objects, even if they are specified in [[reference/minio-server/settings/core#mc-conf.compression.extensions|`extensions`]] or [[reference/minio-server/settings/core#mc-conf.compression.mime_types|`mime_types`]] arguments.
See [Excluded File Types](#minio-data-compression-excluded-types) for a list of excluded types.

The sections below describe how to configure compression for the desired file extensions and media types.

#### Compress All Compressible Objects

To compress all objects except the [default excluded types](#minio-data-compression-excluded-types), use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set the [[reference/minio-server/settings/core#mc-conf.compression|`compression`]] key [[reference/minio-server/settings/core#mc-conf.compression.extensions|`extensions`]] and [[reference/minio-server/settings/core#mc-conf.compression.mime_types|`mime_types`]] options to empty lists:

```
mc admin config set ALIAS compression extensions= mime_types=
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

#### Compress Objects by File Extension

To compress objects with certain file extensions, use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set the desired file extensions in an [[reference/minio-server/settings/core#mc-conf.compression.extensions|`extensions`]] argument.

The following command compresses files with the extensions `.bin` and `.txt`:

```
mc admin config set ALIAS compression extensions=".bin, .txt"
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

The new list of file extensions replaces the previous list.
To add or remove an extension, repeat the [[reference/minio-server/settings/core#mc-conf.compression.extensions|`extensions`]] command with the complete list of extensions to compress.

The following adds `.pdf` to the list of file extensions from the previous example:

```
mc admin config set ALIAS compression extensions=".bin, .txt, .pdf"
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

#### Compress Objects by Media Type

To compress objects of certain media types, use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set the [[reference/minio-server/settings/core#mc-conf.compression|`compression`]] key [[reference/minio-server/settings/core#mc-conf.compression.mime_types|`mime_types`]] option to a list of the desired types.

The following example compresses files of types `application/json` and `image/bmp`:

```
mc admin config set ALIAS compression mime_types="application/json, image/bmp"
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

The new list of media types replaces the previous list.
To add or remove a type, repeat the [[reference/minio-server/settings/core#mc-conf.compression.mime_types|`mime_types`]] command with the complete list of types to compress.

You can use `*` to specify all subtypes of a single media type.
The following command adds all `text` subtypes to the list from the previous example:

```
mc admin config set ALIAS compression mime_types="application/json, image/bmp, text/*"
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).