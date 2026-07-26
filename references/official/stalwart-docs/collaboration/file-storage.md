---
title: "File Storage"
source: "https://stalw.art/docs/collaboration/file-storage"
source_format: official-html-to-markdown
language: "en"
doc_slug: "collaboration/file-storage"
category: "collaboration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "File Storage"
---
# File Storage

      

Stalwart includes support for file storage and remote file management through the JMAP for File Storage and WebDAV protocols, allowing accounts to store, organise, and access files directly.

## JMAP for File Storage

[Section titled “JMAP for File Storage”](#jmap-for-file-storage)

JMAP for File Storage (also known as JMAP for FileNode) is a JMAP-based replacement for traditional WebDAV file storage. It allows clients to manage files and folders (upload, download, move, rename, share) using the same JMAP framework as Mail, Calendars, and Contacts.

Instead of WebDAV’s XML-based PROPFIND/REPORT operations and custom HTTP methods, JMAP for File Storage exposes a JSON data model for files and collections. It integrates with JMAP Sharing and access-control mechanisms.

## WebDAV

[Section titled “WebDAV”](#webdav)

With WebDAV, an account can be connected as a remote drive, so that files can be browsed, uploaded, downloaded, and managed from standard file manager applications on desktop and mobile devices. This provides a protocol-neutral way to share documents, access files remotely, or centralise data storage.

File storage is integrated with the server’s existing resource management features. The same quota system used for email storage also applies to WebDAV file storage, so usage policies remain consistent across services. Total storage consumption can be controlled per account or per tenant.

### Accessing files

[Section titled “Accessing files”](#accessing-files)

Accounts access file storage through the WebDAV protocol at a consistent URL path. Each account’s file storage area is located under `/dav/file/<account_name>`. For example, the account `jane` has its root file storage directory at `/dav/file/jane`.

Clients connect to this path using any WebDAV-compatible file manager or application to browse directories, upload and download files, and organise content. There is no autodiscovery mechanism for file storage, so clients must be configured with the correct path. Once connected, file operations are handled using standard WebDAV methods such as `GET`, `PUT`, `DELETE`, and `MKCOL`.

## Limits

[Section titled “Limits”](#limits)

File storage limits are configured on the [[ref/object/file-storage|FileStorage]] singleton (found in the WebUI under  Settings ›  Files & Sharing › File Storage).

The [[ref/object/file-storage#maxsize|`maxSize`]] field defines the maximum accepted size of a single file submitted to the server. The default is `"25mb"`. Raise or lower the limit to match deployment-specific needs.

The singleton also sets default caps on per-account file resources: [[ref/object/file-storage#maxfiles|`maxFiles`]] caps the number of files an account can create, and [[ref/object/file-storage#maxfolders|`maxFolders`]] caps the number of folders. Both are optional and apply unless overridden at the account or tenant level.