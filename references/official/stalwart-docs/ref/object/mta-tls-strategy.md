---
title: "MtaTlsStrategy"
source: "https://stalw.art/docs/ref/object/mta-tls-strategy"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-tls-strategy"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaTlsStrategy"
---
# MtaTlsStrategy

      

Defines a TLS security strategy for outbound connections.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Outbound › TLS Strategies

## Fields

[Section titled “Fields”](#fields)

##### name

[Section titled “name”](#name)

Type: `String` · read-only

Short identifier for the TLS strategy

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts)

Type: `Boolean` · default: `false`

Whether to allow connections to servers with invalid TLS certificates

##### dane

[Section titled “dane”](#dane)

Type: [`MtaRequiredOrOptional`](#mtarequiredoroptional) · default: `"optional"`

Whether DANE is required, optional, or disabled

##### description

[Section titled “description”](#description)

Type: `String?`

A short description of the TLS strategy, which can be used to identify it in the list of strategies

##### mtaSts

[Section titled “mtaSts”](#mtasts)

Type: [`MtaRequiredOrOptional`](#mtarequiredoroptional) · default: `"optional"`

Whether MTA-STS is required, optional, or disabled

##### startTls

[Section titled “startTls”](#starttls)

Type: [`MtaRequiredOrOptional`](#mtarequiredoroptional) · default: `"optional"`

Whether TLS support is required, optional, or disabled

##### mtaStsTimeout

[Section titled “mtaStsTimeout”](#mtaststimeout)

Type: `Duration` · default: `"5m"`

Maximum time to wait for the MTA-STS policy lookup to complete

##### tlsTimeout

[Section titled “tlsTimeout”](#tlstimeout)

Type: `Duration` · default: `"3m"`

Maximum time to wait for the TLS handshake to complete

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaTlsStrategy object is available via the `urn:stalwart:jmap` capability.

### x:MtaTlsStrategy/get

[Section titled “x:MtaTlsStrategy/get”](#xmtatlsstrategyget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMtaTlsStrategyGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaTlsStrategy/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaTlsStrategy/set

[Section titled “x:MtaTlsStrategy/set”](#xmtatlsstrategyset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMtaTlsStrategyCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaTlsStrategy/set",          {            "create": {              "new1": {}            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMtaTlsStrategyUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaTlsStrategy/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMtaTlsStrategyDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaTlsStrategy/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaTlsStrategy/query

[Section titled “x:MtaTlsStrategy/query”](#xmtatlsstrategyquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMtaTlsStrategyQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaTlsStrategy/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:MtaTlsStrategy/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaTlsStrategy id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MtaTlsStrategy
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MtaTlsStrategystalwart-cli query MtaTlsStrategy --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MtaTlsStrategy id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MtaTlsStrategy --ids id1
```

## Enums

[Section titled “Enums”](#enums)

### MtaRequiredOrOptional

[Section titled “MtaRequiredOrOptional”](#mtarequiredoroptional)
ValueLabel`optional`Optional`require`Required`disable`Disabled