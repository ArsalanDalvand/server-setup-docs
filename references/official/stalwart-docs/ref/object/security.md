---
title: "Security"
source: "https://stalw.art/docs/ref/object/security"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/security"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Security"
---
# Security

      

Configures automatic IP banning rules for abuse, authentication failures, and port scanning.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Security › Settings

## Fields

[Section titled “Fields”](#fields)

##### abuseBanRate

[Section titled “abuseBanRate”](#abusebanrate)

Type: [`Rate`](#rate)`?` · default: `{"count":35,"period":"1d"}`

The maximum number of abuse attempts (relaying or failed RCPT TO attempts) before the IP is banned

##### abuseBanPeriod

[Section titled “abuseBanPeriod”](#abusebanperiod)

Type: `Duration?`

The duration of the ban for abuse attempts

##### authBanRate

[Section titled “authBanRate”](#authbanrate)

Type: [`Rate`](#rate)`?` · default: `{"count":100,"period":"1d"}`

The maximum number of failed login attempts before the IP is banned

##### authBanPeriod

[Section titled “authBanPeriod”](#authbanperiod)

Type: `Duration?`

The duration of the ban for failed login attempts

##### loiterBanRate

[Section titled “loiterBanRate”](#loiterbanrate)

Type: [`Rate`](#rate)`?` · default: `{"count":150,"period":"1d"}`

The maximum number of loitering disconnections before the IP is banned

##### loiterBanPeriod

[Section titled “loiterBanPeriod”](#loiterbanperiod)

Type: `Duration?`

The duration of the ban for loitering connections.

##### scanBanPaths

[Section titled “scanBanPaths”](#scanbanpaths)

Type: `String[]` · default: `["*.php*","*.cgi*","*.asp*","*/wp-*","*/php*","*/cgi-bin*","*xmlrpc*","*../*","*/..*","*joomla*","*wordpress*","*drupal*"]`

The paths that will trigger an immediate ban if accessed. Each path should be a glob expression

##### scanBanRate

[Section titled “scanBanRate”](#scanbanrate)

Type: [`Rate`](#rate)`?` · default: `{"count":30,"period":"1d"}`

The maximum number of port scanning attempts before the IP is banned

##### scanBanPeriod

[Section titled “scanBanPeriod”](#scanbanperiod)

Type: `Duration?`

The duration of the ban for port scanning attempts

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Security singleton is available via the `urn:stalwart:jmap` capability.

### x:Security/get

[Section titled “x:Security/get”](#xsecurityget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSecurityGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Security/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Security/set

[Section titled “x:Security/set”](#xsecurityset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSecurityUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Security/set",          {            "update": {              "singleton": {                "abuseBanRate": {                  "count": 35,                  "period": "1d"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Security
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Security --field abuseBanRate='{"count":35,"period":"1d"}'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Rate

[Section titled “Rate”](#rate)

Defines a rate limit as a count over a time period.

##### count

[Section titled “count”](#count)

Type: `UnsignedInt` · default: `0` · min: 1 · max: 1000000

Count

##### period

[Section titled “period”](#period)

Type: `Duration` · default: `"0s"` · min: 1

Period