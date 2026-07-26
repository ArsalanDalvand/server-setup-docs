---
title: "SieveSystemScript"
source: "https://stalw.art/docs/ref/object/sieve-system-script"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/sieve-system-script"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SieveSystemScript"
---
# SieveSystemScript

      

Defines a system Sieve script executed by the server.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Sieve › System Scripts

## Fields

[Section titled “Fields”](#fields)

##### name

[Section titled “name”](#name)

Type: `String` · required

Name of the script

##### description

[Section titled “description”](#description)

Type: `String?`

Description of the script

##### isActive

[Section titled “isActive”](#isactive)

Type: `Boolean` · default: `false`

Whether the script is active

##### contents

[Section titled “contents”](#contents)

Type: `Text` · required

Content of the script

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SieveSystemScript object is available via the `urn:stalwart:jmap` capability.

### x:SieveSystemScript/get

[Section titled “x:SieveSystemScript/get”](#xsievesystemscriptget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysSieveSystemScriptGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveSystemScript/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SieveSystemScript/set

[Section titled “x:SieveSystemScript/set”](#xsievesystemscriptset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysSieveSystemScriptCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveSystemScript/set",          {            "create": {              "new1": {                "contents": "Example",                "name": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysSieveSystemScriptUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveSystemScript/set",          {            "update": {              "id1": {                "name": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysSieveSystemScriptDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveSystemScript/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SieveSystemScript/query

[Section titled “x:SieveSystemScript/query”](#xsievesystemscriptquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysSieveSystemScriptQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveSystemScript/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:SieveSystemScript/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SieveSystemScript id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create SieveSystemScript \  --field name=Example \  --field contents=Example
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query SieveSystemScriptstalwart-cli query SieveSystemScript --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update SieveSystemScript id1 --field name='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete SieveSystemScript --ids id1
```