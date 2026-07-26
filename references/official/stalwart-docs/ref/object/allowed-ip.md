---
title: "AllowedIp"
source: "https://stalw.art/docs/ref/object/allowed-ip"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/allowed-ip"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "AllowedIp"
---
# AllowedIp

      

Defines an allowed IP address or network range.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Security › Allowed IPs

## Fields

[Section titled “Fields”](#fields)

##### address

[Section titled “address”](#address)

Type: `IpMask` · read-only

The IP address or mask to allow

##### reason

[Section titled “reason”](#reason)

Type: `String?`

The reason for allowing this IP address

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · read-only

The date and time when this IP address was allowed

##### expiresAt

[Section titled “expiresAt”](#expiresat)

Type: `UTCDateTime?`

The date and time when this IP address allowance will expire

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The AllowedIp object is available via the `urn:stalwart:jmap` capability.

### x:AllowedIp/get

[Section titled “x:AllowedIp/get”](#xallowedipget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysAllowedIpGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AllowedIp/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:AllowedIp/set

[Section titled “x:AllowedIp/set”](#xallowedipset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysAllowedIpCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AllowedIp/set",          {            "create": {              "new1": {}            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysAllowedIpUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AllowedIp/set",          {            "update": {              "id1": {                "reason": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysAllowedIpDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AllowedIp/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:AllowedIp/query

[Section titled “x:AllowedIp/query”](#xallowedipquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysAllowedIpQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:AllowedIp/query",          {            "filter": {              "address": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:AllowedIp/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`address`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get AllowedIp id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create AllowedIp
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query AllowedIpstalwart-cli query AllowedIp --where address=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update AllowedIp id1 --field reason='updated value'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete AllowedIp --ids id1
```