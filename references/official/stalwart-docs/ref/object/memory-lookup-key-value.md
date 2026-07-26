---
title: "MemoryLookupKeyValue"
source: "https://stalw.art/docs/ref/object/memory-lookup-key-value"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/memory-lookup-key-value"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MemoryLookupKeyValue"
---
# MemoryLookupKeyValue

      

Defines an in-memory lookup key-value pair.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Lookups › In-Memory Key-Values

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

##### value

[Section titled “value”](#value)

Type: `String` · required

The key value

##### isGlobPattern

[Section titled “isGlobPattern”](#isglobpattern)

Type: `Boolean` · default: `false`

Whether the key is a glob pattern

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MemoryLookupKeyValue object is available via the `urn:stalwart:jmap` capability.

### x:MemoryLookupKeyValue/get

[Section titled “x:MemoryLookupKeyValue/get”](#xmemorylookupkeyvalueget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMemoryLookupKeyValueGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MemoryLookupKeyValue/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MemoryLookupKeyValue/set

[Section titled “x:MemoryLookupKeyValue/set”](#xmemorylookupkeyvalueset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMemoryLookupKeyValueCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MemoryLookupKeyValue/set",          {            "create": {              "new1": {                "key": "Example",                "namespace": "Example",                "value": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMemoryLookupKeyValueUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MemoryLookupKeyValue/set",          {            "update": {              "id1": {                "namespace": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMemoryLookupKeyValueDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MemoryLookupKeyValue/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MemoryLookupKeyValue/query

[Section titled “x:MemoryLookupKeyValue/query”](#xmemorylookupkeyvaluequery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMemoryLookupKeyValueQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MemoryLookupKeyValue/query",          {            "filter": {              "namespace": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:MemoryLookupKeyValue/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`namespace`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MemoryLookupKeyValue id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MemoryLookupKeyValue \  --field namespace=Example \  --field key=Example \  --field value=Example
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MemoryLookupKeyValuestalwart-cli query MemoryLookupKeyValue --where namespace=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MemoryLookupKeyValue id1 --field namespace='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MemoryLookupKeyValue --ids id1
```