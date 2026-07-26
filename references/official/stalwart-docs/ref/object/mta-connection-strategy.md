---
title: "MtaConnectionStrategy"
source: "https://stalw.art/docs/ref/object/mta-connection-strategy"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-connection-strategy"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaConnectionStrategy"
---
# MtaConnectionStrategy

      

Defines a connection strategy for outbound message delivery.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Outbound › Connection Strategies

## Fields

[Section titled “Fields”](#fields)

##### name

[Section titled “name”](#name)

Type: `String` · read-only

Short identifier for the strategy

##### description

[Section titled “description”](#description)

Type: `String?`

Short description of the connection strategy

##### ehloHostname

[Section titled “ehloHostname”](#ehlohostname)

Type: `HostName?`

Overrides the EHLO hostname that will be used when connecting using this strategy

##### sourceIps

[Section titled “sourceIps”](#sourceips)

Type: [`MtaConnectionIpHost`](#mtaconnectioniphost)`[]`

List of local IPv4 and IPv6 addresses to use when delivering emails to remote SMTP servers

##### connectTimeout

[Section titled “connectTimeout”](#connecttimeout)

Type: `Duration` · default: `"5m"`

Maximum time to wait for the connection to be established

##### dataTimeout

[Section titled “dataTimeout”](#datatimeout)

Type: `Duration` · default: `"10m"`

Maximum time to wait for the DATA command response

##### ehloTimeout

[Section titled “ehloTimeout”](#ehlotimeout)

Type: `Duration` · default: `"5m"`

Maximum time to wait for the EHLO command response

##### greetingTimeout

[Section titled “greetingTimeout”](#greetingtimeout)

Type: `Duration` · default: `"5m"`

Maximum time to wait for the SMTP greeting message

##### mailFromTimeout

[Section titled “mailFromTimeout”](#mailfromtimeout)

Type: `Duration` · default: `"5m"`

Maximum time to wait for the MAIL-FROM command response

##### rcptToTimeout

[Section titled “rcptToTimeout”](#rcpttotimeout)

Type: `Duration` · default: `"5m"`

Maximum time to wait for the RCPT-TO command response

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaConnectionStrategy object is available via the `urn:stalwart:jmap` capability.

### x:MtaConnectionStrategy/get

[Section titled “x:MtaConnectionStrategy/get”](#xmtaconnectionstrategyget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysMtaConnectionStrategyGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaConnectionStrategy/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaConnectionStrategy/set

[Section titled “x:MtaConnectionStrategy/set”](#xmtaconnectionstrategyset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysMtaConnectionStrategyCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaConnectionStrategy/set",          {            "create": {              "new1": {                "sourceIps": {}              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysMtaConnectionStrategyUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaConnectionStrategy/set",          {            "update": {              "id1": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysMtaConnectionStrategyDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaConnectionStrategy/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaConnectionStrategy/query

[Section titled “x:MtaConnectionStrategy/query”](#xmtaconnectionstrategyquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysMtaConnectionStrategyQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaConnectionStrategy/query",          {            "filter": {              "name": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:MtaConnectionStrategy/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`name`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaConnectionStrategy id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create MtaConnectionStrategy \  --field 'sourceIps={}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query MtaConnectionStrategystalwart-cli query MtaConnectionStrategy --where name=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update MtaConnectionStrategy id1 --field description='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete MtaConnectionStrategy --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### MtaConnectionIpHost

[Section titled “MtaConnectionIpHost”](#mtaconnectioniphost)

Defines a source IP address and optional EHLO hostname override for outbound connections.

##### ehloHostname

[Section titled “ehloHostname”](#ehlohostname-1)

Type: `HostName?`

Overrides the EHLO hostname that will be used when connecting from this IP address

##### sourceIp

[Section titled “sourceIp”](#sourceip)

Type: `IpAddr` · required

Local IPv4 and IPv6 address to use when delivering emails to remote SMTP servers