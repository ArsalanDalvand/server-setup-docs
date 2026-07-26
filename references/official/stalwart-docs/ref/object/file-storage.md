---
title: "FileStorage"
source: "https://stalw.art/docs/ref/object/file-storage"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/file-storage"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "FileStorage"
---
# FileStorage

      

Configures file storage limits.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Files & Sharing › File Storage

## Fields

[Section titled “Fields”](#fields)

##### maxSize

[Section titled “maxSize”](#maxsize)

Type: `Size` · default: `"25mb"`

Specifies the maximum size of a file that can be uploaded to the server

##### maxFiles

[Section titled “maxFiles”](#maxfiles)

Type: `UnsignedInt?` · min: 1

The default maximum number of files a user can create

##### maxFolders

[Section titled “maxFolders”](#maxfolders)

Type: `UnsignedInt?` · min: 1

The default maximum number of file folders a user can create

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The FileStorage singleton is available via the `urn:stalwart:jmap` capability.

### x:FileStorage/get

[Section titled “x:FileStorage/get”](#xfilestorageget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysFileStorageGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:FileStorage/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:FileStorage/set

[Section titled “x:FileStorage/set”](#xfilestorageset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysFileStorageUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:FileStorage/set",          {            "update": {              "singleton": {                "maxSize": "25mb"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get FileStorage
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update FileStorage --field maxSize=25mb
```