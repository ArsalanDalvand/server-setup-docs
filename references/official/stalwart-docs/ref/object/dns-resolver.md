---
title: "DnsResolver"
source: "https://stalw.art/docs/ref/object/dns-resolver"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/dns-resolver"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DnsResolver"
---
# DnsResolver

      

Configures the DNS resolver used for domain lookups.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Network › DNS › DNS Resolver

## Fields

[Section titled “Fields”](#fields)

DnsResolver is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "System"

[Section titled “@type: "System"”](#type-system)

System Resolver

##### attempts

[Section titled “attempts”](#attempts)

Type: `UnsignedInt` · default: `2`

Number of times a resolution request will be retried before it is considered failed

##### concurrency

[Section titled “concurrency”](#concurrency)

Type: `UnsignedInt` · default: `2`

Number of concurrent resolution requests that can be made at the same time

##### enableEdns

[Section titled “enableEdns”](#enableedns)

Type: `Boolean` · default: `true`

Whether to enable EDNS (Extension Mechanisms for DNS) support

##### preserveIntermediates

[Section titled “preserveIntermediates”](#preserveintermediates)

Type: `Boolean` · default: `true`

Whether to preserve the intermediate name servers in the DNS resolution results

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"5s"`

Time after which a resolution request will be timed out if no response is received

##### tcpOnError

[Section titled “tcpOnError”](#tcponerror)

Type: `Boolean` · default: `true`

Whether to try using TCP for resolution requests if an error occurs during a UDP resolution request

### @type: "Custom"

[Section titled “@type: "Custom"”](#type-custom)

Custom DNS

##### servers

[Section titled “servers”](#servers)

Type: [`DnsCustomResolver`](#dnscustomresolver)`[]` · min items: 1

List of custom DNS server URLs to use for resolution

##### attempts

[Section titled “attempts”](#attempts-1)

Type: `UnsignedInt` · default: `2`

Number of times a resolution request will be retried before it is considered failed

##### concurrency

[Section titled “concurrency”](#concurrency-1)

Type: `UnsignedInt` · default: `2`

Number of concurrent resolution requests that can be made at the same time

##### enableEdns

[Section titled “enableEdns”](#enableedns-1)

Type: `Boolean` · default: `true`

Whether to enable EDNS (Extension Mechanisms for DNS) support

##### preserveIntermediates

[Section titled “preserveIntermediates”](#preserveintermediates-1)

Type: `Boolean` · default: `true`

Whether to preserve the intermediate name servers in the DNS resolution results

##### timeout

[Section titled “timeout”](#timeout-1)

Type: `Duration` · default: `"5s"`

Time after which a resolution request will be timed out if no response is received

##### tcpOnError

[Section titled “tcpOnError”](#tcponerror-1)

Type: `Boolean` · default: `true`

Whether to try using TCP for resolution requests if an error occurs during a UDP resolution request

### @type: "Cloudflare"

[Section titled “@type: "Cloudflare"”](#type-cloudflare)

Cloudflare DNS

##### useTls

[Section titled “useTls”](#usetls)

Type: `Boolean` · default: `true`

Whether to use TLS for DNS resolution

##### attempts

[Section titled “attempts”](#attempts-2)

Type: `UnsignedInt` · default: `2`

Number of times a resolution request will be retried before it is considered failed

##### concurrency

[Section titled “concurrency”](#concurrency-2)

Type: `UnsignedInt` · default: `2`

Number of concurrent resolution requests that can be made at the same time

##### enableEdns

[Section titled “enableEdns”](#enableedns-2)

Type: `Boolean` · default: `true`

Whether to enable EDNS (Extension Mechanisms for DNS) support

##### preserveIntermediates

[Section titled “preserveIntermediates”](#preserveintermediates-2)

Type: `Boolean` · default: `true`

Whether to preserve the intermediate name servers in the DNS resolution results

##### timeout

[Section titled “timeout”](#timeout-2)

Type: `Duration` · default: `"5s"`

Time after which a resolution request will be timed out if no response is received

##### tcpOnError

[Section titled “tcpOnError”](#tcponerror-2)

Type: `Boolean` · default: `true`

Whether to try using TCP for resolution requests if an error occurs during a UDP resolution request

### @type: "Quad9"

[Section titled “@type: "Quad9"”](#type-quad9)

Quad9 DNS

##### useTls

[Section titled “useTls”](#usetls-1)

Type: `Boolean` · default: `true`

Whether to use TLS for DNS resolution

##### attempts

[Section titled “attempts”](#attempts-3)

Type: `UnsignedInt` · default: `2`

Number of times a resolution request will be retried before it is considered failed

##### concurrency

[Section titled “concurrency”](#concurrency-3)

Type: `UnsignedInt` · default: `2`

Number of concurrent resolution requests that can be made at the same time

##### enableEdns

[Section titled “enableEdns”](#enableedns-3)

Type: `Boolean` · default: `true`

Whether to enable EDNS (Extension Mechanisms for DNS) support

##### preserveIntermediates

[Section titled “preserveIntermediates”](#preserveintermediates-3)

Type: `Boolean` · default: `true`

Whether to preserve the intermediate name servers in the DNS resolution results

##### timeout

[Section titled “timeout”](#timeout-3)

Type: `Duration` · default: `"5s"`

Time after which a resolution request will be timed out if no response is received

##### tcpOnError

[Section titled “tcpOnError”](#tcponerror-3)

Type: `Boolean` · default: `true`

Whether to try using TCP for resolution requests if an error occurs during a UDP resolution request

### @type: "Google"

[Section titled “@type: "Google"”](#type-google)

Google DNS

##### attempts

[Section titled “attempts”](#attempts-4)

Type: `UnsignedInt` · default: `2`

Number of times a resolution request will be retried before it is considered failed

##### concurrency

[Section titled “concurrency”](#concurrency-4)

Type: `UnsignedInt` · default: `2`

Number of concurrent resolution requests that can be made at the same time

##### enableEdns

[Section titled “enableEdns”](#enableedns-4)

Type: `Boolean` · default: `true`

Whether to enable EDNS (Extension Mechanisms for DNS) support

##### preserveIntermediates

[Section titled “preserveIntermediates”](#preserveintermediates-4)

Type: `Boolean` · default: `true`

Whether to preserve the intermediate name servers in the DNS resolution results

##### timeout

[Section titled “timeout”](#timeout-4)

Type: `Duration` · default: `"5s"`

Time after which a resolution request will be timed out if no response is received

##### tcpOnError

[Section titled “tcpOnError”](#tcponerror-4)

Type: `Boolean` · default: `true`

Whether to try using TCP for resolution requests if an error occurs during a UDP resolution request

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The DnsResolver singleton is available via the `urn:stalwart:jmap` capability.

### x:DnsResolver/get

[Section titled “x:DnsResolver/get”](#xdnsresolverget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysDnsResolverGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DnsResolver/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DnsResolver/set

[Section titled “x:DnsResolver/set”](#xdnsresolverset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysDnsResolverUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DnsResolver/set",          {            "update": {              "singleton": {                "attempts": 2              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get DnsResolver
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update DnsResolver --field attempts=2
```

## Nested types

[Section titled “Nested types”](#nested-types)

### DnsCustomResolver

[Section titled “DnsCustomResolver”](#dnscustomresolver)

Custom DNS server endpoint.

##### protocol

[Section titled “protocol”](#protocol)

Type: [`DnsResolverProtocol`](#dnsresolverprotocol) · default: `"udp"`

Protocol to use for DNS queries

##### address

[Section titled “address”](#address)

Type: `IpAddr` · default: `"127.0.0.1"`

Address of the DNS server

##### port

[Section titled “port”](#port)

Type: `UnsignedInt` · default: `53` · min: 1 · max: 65535

Port of the DNS server

## Enums

[Section titled “Enums”](#enums)

### DnsResolverProtocol

[Section titled “DnsResolverProtocol”](#dnsresolverprotocol)
ValueLabel`tls`DNS over TLS`udp`UDP`tcp`TCP