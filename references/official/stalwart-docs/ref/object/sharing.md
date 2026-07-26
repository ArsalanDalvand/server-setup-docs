---
title: "Sharing"
source: "https://stalw.art/docs/ref/object/sharing"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/sharing"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Sharing"
---
# Sharing

      

Configures sharing settings for calendars, address books, and files.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Files & Sharing › Sharing

## Fields

[Section titled “Fields”](#fields)

##### allowDirectoryQueries

[Section titled “allowDirectoryQueries”](#allowdirectoryqueries)

Type: `Boolean` · default: `false`

Whether authenticated users can query the directory via WebDAV and JMAP

##### maxShares

[Section titled “maxShares”](#maxshares)

Type: `UnsignedInt` · default: `10` · min: 1

Specifies the maximum number of sharees that can be added to a single shared item (calendar, address book or file)

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Sharing singleton is available via the `urn:stalwart:jmap` capability.

### x:Sharing/get

[Section titled “x:Sharing/get”](#xsharingget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSharingGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Sharing/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Sharing/set

[Section titled “x:Sharing/set”](#xsharingset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSharingUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Sharing/set",          {            "update": {              "singleton": {                "allowDirectoryQueries": false              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Sharing
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Sharing --field allowDirectoryQueries=false
```