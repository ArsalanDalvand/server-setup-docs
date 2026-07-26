---
title: "SpamFileExtension"
source: "https://stalw.art/docs/ref/object/spam-file-extension"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spam-file-extension"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpamFileExtension"
---
# SpamFileExtension

      

Defines a file extension classification rule for spam filtering.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Spam Filter › Lists › File Extensions

## Fields

[Section titled “Fields”](#fields)

##### extension

[Section titled “extension”](#extension)

Type: `String` · read-only

The file name extension

##### isArchive

[Section titled “isArchive”](#isarchive)

Type: `Boolean` · default: `false`

Whether this file extension is considered an archive

##### isBad

[Section titled “isBad”](#isbad)

Type: `Boolean` · default: `false`

Whether this file extension is considered bad

##### isNz

[Section titled “isNz”](#isnz)

Type: `Boolean` · default: `false`

Whether this file extension is considered a NZ file

##### contentTypes

[Section titled “contentTypes”](#contenttypes)

Type: `String[]`

The MIME types associated with this file extension

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpamFileExtension object is available via the `urn:stalwart:jmap` capability.

### x:SpamFileExtension/get

[Section titled “x:SpamFileExtension/get”](#xspamfileextensionget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysSpamFileExtensionGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamFileExtension/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamFileExtension/set

[Section titled “x:SpamFileExtension/set”](#xspamfileextensionset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysSpamFileExtensionCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamFileExtension/set",          {            "create": {              "new1": {                "contentTypes": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysSpamFileExtensionUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamFileExtension/set",          {            "update": {              "id1": {                "isArchive": false              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysSpamFileExtensionDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamFileExtension/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamFileExtension/query

[Section titled “x:SpamFileExtension/query”](#xspamfileextensionquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysSpamFileExtensionQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamFileExtension/query",          {            "filter": {              "extension": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:SpamFileExtension/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`extension`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpamFileExtension id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create SpamFileExtension \  --field 'contentTypes={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query SpamFileExtensionstalwart-cli query SpamFileExtension --where extension=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update SpamFileExtension id1 --field isArchive=false
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete SpamFileExtension --ids id1
```