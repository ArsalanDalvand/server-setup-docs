---
title: "MtaSts"
source: "https://stalw.art/docs/ref/object/mta-sts"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/mta-sts"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "MtaSts"
---
# MtaSts

      

Configures the MTA-STS policy for the server.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  MTA › Inbound › MTA-STS

## Fields

[Section titled “Fields”](#fields)

##### maxAge

[Section titled “maxAge”](#maxage)

Type: `Duration` · default: `"7d"`

Maximum time to cache the MTA-STS policy

##### mode

[Section titled “mode”](#mode)

Type: [`PolicyEnforcement`](#policyenforcement) · default: `"testing"`

Whether to enforce, test, or disable the MTA-STS policy

##### mxHosts

[Section titled “mxHosts”](#mxhosts)

Type: `String[]`

Override the allowed MX hosts for the MTA-STS policy domain. If empty, the MX hosts are determined from the system settings

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The MtaSts singleton is available via the `urn:stalwart:jmap` capability.

### x:MtaSts/get

[Section titled “x:MtaSts/get”](#xmtastsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysMtaStsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaSts/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:MtaSts/set

[Section titled “x:MtaSts/set”](#xmtastsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysMtaStsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:MtaSts/set",          {            "update": {              "singleton": {                "maxAge": "7d"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get MtaSts
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update MtaSts --field maxAge=7d
```

## Enums

[Section titled “Enums”](#enums)

### PolicyEnforcement

[Section titled “PolicyEnforcement”](#policyenforcement)
ValueLabel`enforce`Enforce`testing`Testing`disable`Disabled