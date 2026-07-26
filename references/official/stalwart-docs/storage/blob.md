---
title: "Blob store"
source: "https://stalw.art/docs/storage/blob"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/blob"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Blob store"
---
# Blob store

      

The blob store holds the large binary objects managed by the server: raw email messages, Sieve scripts, and other files. Every blob is addressed by a content hash (BLAKE3) derived from the object’s bytes, so two uploads with identical content share a single physical object. This content-addressed model enables straightforward deduplication across users and mailboxes.

Backend selection is made on the [[ref/object/blob-store|BlobStore]] singleton (found in the WebUI under  Settings ›  Storage › Blob Store). The object is a multi-variant type; each variant carries its own fields. The supported variants are:

[[storage/backends/s3|S3-compatible storage]]: for Amazon S3, Google Cloud Storage, MinIO, and other S3-compatible services.
[[storage/backends/azure|Azure Blob Storage]]: for the Azure-native object storage service.
[[storage/backends/filesystem|Filesystem]]: direct storage on the server’s local filesystem, suitable for single-node installations.
The `Default` variant, which reuses the configured data store as the blob store; useful for installations that prefer to minimise the number of external dependencies.
For distributed installations, FoundationDB, PostgreSQL, and MySQL are also available as blob store variants; configuration mirrors the equivalent DataStore variants.

## Configuration

[Section titled “Configuration”](#configuration)

To change the blob store backend, update the BlobStore singleton and select the appropriate variant. Variant-specific fields such as [[ref/object/blob-store#bucket|`bucket`]], [[ref/object/blob-store#region|`region`]], and [[ref/object/blob-store#secretkey|`secretKey`]] apply to the S3 variant; [[ref/object/blob-store#path|`path`]] and [[ref/object/blob-store#depth|`depth`]] apply to the FileSystem variant; and so on. See the [[ref/object/blob-store|BlobStore reference]] for the full field list per variant.

## Compression

[Section titled “Compression”](#compression)

Blob compression is controlled through the [[ref/object/email|Email]] object (found in the WebUI under  Settings ›  Email › Defaults,  Settings ›  Email › Encryption,  Settings ›  Email › Limits,  Settings ›  Email › Storage) via the [[ref/object/email#compressionalgorithm|`compressionAlgorithm`]] field. The setting applies to every blob written to the store (messages, attachments, Sieve scripts, and any other stored binary). The default is LZ4. Setting the field to `"none"` disables compression; setting it to `"lz4"` enables [LZ4](https://en.wikipedia.org/wiki/LZ4_(compression_algorithm)) compression.

## Maintenance

[Section titled “Maintenance”](#maintenance)

Temporary blobs uploaded through the JMAP [[http/jmap/protocol#upload-limits|upload endpoint]] are retained for a limited period (their Time-To-Live). Once the TTL expires without the blob being referenced, the server’s scheduled clean-up task removes them, reclaiming storage.

The clean-up schedule is managed through the [[ref/object/data-retention|DataRetention]] object (found in the WebUI under  Settings ›  Storage › Data Retention › Archiving,  Settings ›  Storage › Data Retention › Data Cleanup,  Settings ›  Storage › Data Retention › Auto-Expunge,  Settings ›  Storage › Data Retention › Telemetry) via the [[ref/object/data-retention#blobcleanupschedule|`blobCleanupSchedule`]] field, which accepts a cron expression. The default runs daily at 04:00.