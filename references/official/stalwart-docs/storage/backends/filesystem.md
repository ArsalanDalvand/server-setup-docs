---
title: "Filesystem"
source: "https://stalw.art/docs/storage/backends/filesystem"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/filesystem"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Filesystem"
---
# Filesystem

      

For single-node installations, Stalwart can store email bodies and other blobs directly on the local filesystem. A hashed directory layout distributes files into nested subdirectories to avoid the performance issues that arise when a single directory contains a very large number of files.

## Configuration

[Section titled “Configuration”](#configuration)

The filesystem backend is selected by choosing the `FileSystem` variant on the [[ref/object/blob-store|BlobStore]] object (found in the WebUI under  Settings ›  Storage › Blob Store). The variant exposes the following fields:

[[ref/object/blob-store#path|`path`]]: filesystem path where blobs are stored (required).
[[ref/object/blob-store#depth|`depth`]]: maximum depth of the nested directory hierarchy. Default: `2`.