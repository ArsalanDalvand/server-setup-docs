---
title: "BlockedIp"
source: "https://stalw.art/docs/ref/object/blocked-ip"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/blocked-ip"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "BlockedIp"
---
# BlockedIp

      

Defines a blocked IP address or network range.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Security › Blocked IPs

## Fields

[Section titled “Fields”](#fields)

##### address

[Section titled “address”](#address)

Type: `IpMask` · read-only

The IP address or mask to block

##### reason

[Section titled “reason”](#reason)

Type: [`BlockReason`](#blockreason) · default: `"manual"`

The reason for blocking this IP address

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · server-set

The date and time when this IP address was blocked

##### expiresAt

[Section titled “expiresAt”](#expiresat)

Type: `UTCDateTime?`

The date and time when this IP address block will expire

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The BlockedIp object is available via the `urn:stalwart:jmap` capability.

### x:BlockedIp/get

[Section titled “x:BlockedIp/get”](#xblockedipget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysBlockedIpGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:BlockedIp/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:BlockedIp/set

[Section titled “x:BlockedIp/set”](#xblockedipset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysBlockedIpCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:BlockedIp/set",          {            "create": {              "new1": {}            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysBlockedIpUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:BlockedIp/set",          {            "update": {              "id1": {                "reason": "manual"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysBlockedIpDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:BlockedIp/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:BlockedIp/query

[Section titled “x:BlockedIp/query”](#xblockedipquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysBlockedIpQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:BlockedIp/query",          {            "filter": {              "address": "example"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:BlockedIp/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`address`text

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get BlockedIp id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create BlockedIp
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query BlockedIpstalwart-cli query BlockedIp --where address=example
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update BlockedIp id1 --field reason=manual
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete BlockedIp --ids id1
```

## Enums

[Section titled “Enums”](#enums)

### BlockReason

[Section titled “BlockReason”](#blockreason)
ValueLabel`rcptToFailure`Excessive failed RCPT TO commands`authFailure`Excessive failed authentication attempts`loitering`Excessive loitering connections`portScanning`Excessive port scanning attempts`manual`Manually blocked IP address`other`Other reason