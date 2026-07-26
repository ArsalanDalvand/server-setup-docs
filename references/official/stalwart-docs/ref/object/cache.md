---
title: "Cache"
source: "https://stalw.art/docs/ref/object/cache"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/cache"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Cache"
---
# Cache

      

Configures in-memory cache sizes for data, DNS records, and authorization tokens.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Storage › Cache

## Fields

[Section titled “Fields”](#fields)

##### accessTokens

[Section titled “accessTokens”](#accesstokens)

Type: `Size` · default: `"10mb"` · min: 2048

Maximum size of the access tokens cache

##### contacts

[Section titled “contacts”](#contacts)

Type: `Size` · default: `"10mb"` · min: 2048

Maximum size of the address books and contacts cache

##### dnsIpv4

[Section titled “dnsIpv4”](#dnsipv4)

Type: `Size` · default: `"5mb"` · min: 2048

Maximum size of the IPv4 record cache

##### dnsIpv6

[Section titled “dnsIpv6”](#dnsipv6)

Type: `Size` · default: `"5mb"` · min: 2048

Maximum size of the IPv6 record cache

##### dnsMtaSts

[Section titled “dnsMtaSts”](#dnsmtasts)

Type: `Size` · default: `"1mb"` · min: 2048

Maximum size of the MTA-STS record cache

##### dnsMx

[Section titled “dnsMx”](#dnsmx)

Type: `Size` · default: `"5mb"` · min: 2048

Maximum size of the MX record cache

##### dnsPtr

[Section titled “dnsPtr”](#dnsptr)

Type: `Size` · default: `"1mb"` · min: 2048

Maximum size of the PTR record cache

##### dnsRbl

[Section titled “dnsRbl”](#dnsrbl)

Type: `Size` · default: `"5mb"` · min: 2048

Maximum size of the DNSBl record cache

##### dnsTlsa

[Section titled “dnsTlsa”](#dnstlsa)

Type: `Size` · default: `"1mb"` · min: 2048

Maximum size of the TLSA record cache

##### dnsTxt

[Section titled “dnsTxt”](#dnstxt)

Type: `Size` · default: `"5mb"` · min: 2048

Maximum size of the TXT record cache

##### events

[Section titled “events”](#events)

Type: `Size` · default: `"10mb"` · min: 2048

Maximum size of the calendar and events cache

##### scheduling

[Section titled “scheduling”](#scheduling)

Type: `Size` · default: `"1mb"` · min: 2048

Maximum size of the scheduling cache

##### files

[Section titled “files”](#files)

Type: `Size` · default: `"10mb"` · min: 2048

Maximum size of the file storage data cache

##### httpAuth

[Section titled “httpAuth”](#httpauth)

Type: `Size` · default: `"1mb"` · min: 2048

Maximum size of the HTTP authorization headers cache

##### messages

[Section titled “messages”](#messages)

Type: `Size` · default: `"50mb"` · min: 2048

Maximum size of the e-mail data cache

##### domains

[Section titled “domains”](#domains)

Type: `Size` · default: `"5mb"` · min: 2048

Maximum size of the domains cache

##### domainNames

[Section titled “domainNames”](#domainnames)

Type: `Size` · default: `"10mb"` · min: 2048

Maximum size of the domain name lookup cache

##### domainNamesNegative

[Section titled “domainNamesNegative”](#domainnamesnegative)

Type: `Size` · default: `"1mb"` · min: 2048

Maximum size of the domain name lookup negative cache

##### emailAddresses

[Section titled “emailAddresses”](#emailaddresses)

Type: `Size` · default: `"10mb"` · min: 2048

Maximum size of the email addresses lookup cache

##### emailAddressesNegative

[Section titled “emailAddressesNegative”](#emailaddressesnegative)

Type: `Size` · default: `"2mb"` · min: 2048

Maximum size of the email addresses lookup negative cache

##### accounts

[Section titled “accounts”](#accounts)

Type: `Size` · default: `"20mb"` · min: 2048

Maximum size of the accounts cache

##### roles

[Section titled “roles”](#roles)

Type: `Size` · default: `"5mb"` · min: 2048

Maximum size of the roles cache

##### tenants

[Section titled “tenants”](#tenants)

Type: `Size` · default: `"5mb"` · min: 2048

Maximum size of the tenants cache

##### mailingLists

[Section titled “mailingLists”](#mailinglists)

Type: `Size` · default: `"2mb"` · min: 2048

Maximum size of the mailing lists cache

##### dkimSignatures

[Section titled “dkimSignatures”](#dkimsignatures)

Type: `Size` · default: `"10mb"` · min: 2048

Maximum size of the DKIM signatures cache

##### negativeTtl

[Section titled “negativeTtl”](#negativettl)

Type: `Duration` · default: `"1h"`

Time-to-live for domain and account name lookup negative cache entries

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Cache singleton is available via the `urn:stalwart:jmap` capability.

### x:Cache/get

[Section titled “x:Cache/get”](#xcacheget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysCacheGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Cache/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Cache/set

[Section titled “x:Cache/set”](#xcacheset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysCacheUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Cache/set",          {            "update": {              "singleton": {                "accessTokens": "10mb"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Cache
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Cache --field accessTokens=10mb
```