---
title: "MemoryLookupKey"
source: "https://stalw.art/docs/ref/object/memory-lookup-key"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/memory-lookup-key"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MemoryLookupKey"
---
# MemoryLookupKey

      

Defines an in-memory lookup key for fast data access.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Lookups › In-Memory Keys Settings ›  Spam Filter › Lists › Blocked Domains Settings ›  Spam Filter › Lists › Spam Traps Settings ›  Spam Filter › Lists › Trusted Domains Settings ›  Spam Filter › Lists › URL Redirectors

## Fields

[Section titled “Fields”](#fields)

##### namespace

[Section titled “namespace”](#namespace)

Type: `String` · required

The namespace of the key

##### key

[Section titled “key”](#key)

Type: `String` · required · max length: 255

The key name

##### isGlobPattern

[Section titled “isGlobPattern”](#isglobpattern)

Type: `Boolean` · default: `false`

Whether the key is a glob pattern

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MemoryLookupKey object is available via the `urn:stalwart:jmap` capability.

### x:MemoryLookupKey/get

[Section titled “x:MemoryLookupKey/get”](#xmemorylookupkeyget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMemoryLookupKeyGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MemoryLookupKey/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MemoryLookupKey/set

[Section titled “x:MemoryLookupKey/set”](#xmemorylookupkeyset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMemoryLookupKeyCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MemoryLookupKey/set",          {            "create": {              "new1": {                "key": "Example",                "namespace": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMemoryLookupKeyUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MemoryLookupKey/set",          {            "update": {              "id1": {                "namespace": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMemoryLookupKeyDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MemoryLookupKey/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MemoryLookupKey/query

[Section titled “x:MemoryLookupKey/query”](#xmemorylookupkeyquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMemoryLookupKeyQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MemoryLookupKey/query",          {            "filter": {              "namespace": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:MemoryLookupKey/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`namespace`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MemoryLookupKey id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MemoryLookupKey \  --field namespace=Example \  --field key=Example
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MemoryLookupKeystalwart-cli query MemoryLookupKey --where namespace=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MemoryLookupKey id1 --field namespace='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MemoryLookupKey --ids id1
```