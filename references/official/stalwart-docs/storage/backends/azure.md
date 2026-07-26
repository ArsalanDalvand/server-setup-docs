---
title: "Azure Blob Storage"
source: "https://stalw.art/docs/storage/backends/azure"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/azure"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Azure Blob Storage"
---
# Azure Blob Storage

      

For distributed and larger-scale deployments, Stalwart supports Azure Blob Storage as a blob store. Azure Blob Storage is a cloud-based object storage service designed for scalable storage of unstructured data, and is well suited to large-scale storage management, replication, and backup of email bodies, Sieve scripts, and other blobs.

## Configuration

[Section titled “Configuration”](#configuration)

The Azure backend is selected by choosing the `Azure` variant on the [[ref/object/blob-store|BlobStore]] object (found in the WebUI under  Settings ›  Storage › Blob Store). The variant exposes the following fields:

[[ref/object/blob-store#storageaccount|`storageAccount`]]: the Azure Storage Account used to hold blobs (required).
[[ref/object/blob-store#container|`container`]]: the container within the Storage Account where blobs are stored (required).
[[ref/object/blob-store#accesskey|`accessKey`]]: the access key for the Storage Account (required). The secret may be supplied inline, read from an environment variable, or loaded from a file.
[[ref/object/blob-store#sastoken|`sasToken`]]: shared access signature token, used instead of an access key for SAS-based authentication.
[[ref/object/blob-store#timeout|`timeout`]]: maximum time to wait for a response from the Azure service. Default: `"30s"`.
[[ref/object/blob-store#maxretries|`maxRetries`]]: maximum number of retries for a failed operation. Default: `3`.
[[ref/object/blob-store#keyprefix|`keyPrefix`]]: optional prefix prepended to every object key, useful for organising or segregating data within a container.