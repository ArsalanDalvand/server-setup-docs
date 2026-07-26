---
title: "SpamPyzor"
source: "https://stalw.art/docs/ref/object/spam-pyzor"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spam-pyzor"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpamPyzor"
---
# SpamPyzor

      

Configures the Pyzor collaborative spam detection service.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Spam Filter › Pyzor

## Fields

[Section titled “Fields”](#fields)

##### blockCount

[Section titled “blockCount”](#blockcount)

Type: `UnsignedInt` · default: `5` · max: 1000 · min: 1

The number of times the hash appears in the Pyzor blocklist

##### enable

[Section titled “enable”](#enable)

Type: `Boolean` · default: `true`

Whether to enable the Pyzor classifier. Pyzor is a collaborative, networked system to detect and report spam.

##### host

[Section titled “host”](#host)

Type: `String` · default: `"public.pyzor.org"`

The hostname of the Pyzor server

##### port

[Section titled “port”](#port)

Type: `UnsignedInt` · default: `24441` · max: 65535 · min: 100

The port to connect to the Pyzor server

##### ratio

[Section titled “ratio”](#ratio)

Type: `Float` · default: `0.2` · max: 1 · min: 0

The ratio of the number of times the hash appears in the Pyzor allowlist to the blocklist

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"5s"`

The timeout for the Pyzor server. If the server does not respond within this time, the check is considered failed.

##### allowCount

[Section titled “allowCount”](#allowcount)

Type: `UnsignedInt` · default: `10` · max: 1000 · min: 1

The number of times the hash appears in the Pyzor allowlist

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpamPyzor singleton is available via the `urn:stalwart:jmap` capability.

### x:SpamPyzor/get

[Section titled “x:SpamPyzor/get”](#xspampyzorget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSpamPyzorGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamPyzor/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamPyzor/set

[Section titled “x:SpamPyzor/set”](#xspampyzorset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSpamPyzorUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamPyzor/set",          {            "update": {              "singleton": {                "host": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpamPyzor
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SpamPyzor --field host='updated value'
```