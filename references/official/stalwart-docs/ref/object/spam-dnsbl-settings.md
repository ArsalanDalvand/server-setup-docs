---
title: "SpamDnsblSettings"
source: "https://stalw.art/docs/ref/object/spam-dnsbl-settings"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/spam-dnsbl-settings"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SpamDnsblSettings"
---
# SpamDnsblSettings

      

Configures DNSBL query limits for spam filtering.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Spam Filter › DNSBL › Settings

## Fields

[Section titled “Fields”](#fields)

##### domainLimit

[Section titled “domainLimit”](#domainlimit)

Type: `UnsignedInt` · default: `50` · min: 1

Maximum number of DNSBL checks for domain names

##### emailLimit

[Section titled “emailLimit”](#emaillimit)

Type: `UnsignedInt` · default: `50` · min: 1

Maximum number of DNSBL checks for E-mail addresses

##### ipLimit

[Section titled “ipLimit”](#iplimit)

Type: `UnsignedInt` · default: `50` · min: 1

Maximum number of DNSBL checks for IP addresses

##### urlLimit

[Section titled “urlLimit”](#urllimit)

Type: `UnsignedInt` · default: `50` · min: 1

Maximum number of DNSBL checks for URLs

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SpamDnsblSettings singleton is available via the `urn:stalwart:jmap` capability.

### x:SpamDnsblSettings/get

[Section titled “x:SpamDnsblSettings/get”](#xspamdnsblsettingsget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSpamDnsblSettingsGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamDnsblSettings/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SpamDnsblSettings/set

[Section titled “x:SpamDnsblSettings/set”](#xspamdnsblsettingsset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSpamDnsblSettingsUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SpamDnsblSettings/set",          {            "update": {              "singleton": {                "domainLimit": 50              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SpamDnsblSettings
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SpamDnsblSettings --field domainLimit=50
```