---
title: "SieveUserScript"
source: "https://stalw.art/docs/ref/object/sieve-user-script"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/sieve-user-script"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SieveUserScript"
---
# SieveUserScript

      

Defines a global Sieve script available for user imports.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Sieve › User Scripts

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

The SieveUserScript object is available via the `urn:stalwart:jmap` capability.

### x:SieveUserScript/get

[Section titled “x:SieveUserScript/get”](#xsieveuserscriptget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysSieveUserScriptGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveUserScript/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SieveUserScript/set

[Section titled “x:SieveUserScript/set”](#xsieveuserscriptset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysSieveUserScriptCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveUserScript/set",          {            "create": {              "new1": {                "contents": "Example",                "name": "Example"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysSieveUserScriptUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveUserScript/set",          {            "update": {              "id1": {                "name": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysSieveUserScriptDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveUserScript/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SieveUserScript/query

[Section titled “x:SieveUserScript/query”](#xsieveuserscriptquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysSieveUserScriptQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SieveUserScript/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:SieveUserScript/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SieveUserScript id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create SieveUserScript \  --field name=Example \  --field contents=Example
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query SieveUserScriptstalwart-cli query SieveUserScript --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update SieveUserScript id1 --field name='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete SieveUserScript --ids id1
```