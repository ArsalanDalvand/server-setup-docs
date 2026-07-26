---
title: "DNS Resolver"
source: "https://stalw.art/docs/server/dns/resolver"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/dns/resolver"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DNS Resolver"
---
# DNS Resolver

      

The DNS resolver translates human-readable domain names into IP addresses and other machine-readable records. It is used throughout the MTA for MX lookups, IP reputation queries, MTA-STS policy retrieval, and many other operations.

The DNS resolver is configured on the [[ref/object/dns-resolver|DnsResolver]] singleton (found in the WebUI under  Settings ›  Network › DNS › DNS Resolver). It is a multi-variant object: each instance selects one of several resolver backends, and the chosen variant determines which fields apply. The supported variants are:

`System`: use the operating-system resolver configured on the host.
`Cloudflare`: use Cloudflare’s public DNS service. A TLS option is available via the variant fields.
`Quad9`: use Quad9’s public DNS service. A TLS option is available via the variant fields.
`Google`: use Google’s public DNS service.
`Custom`: use a list of DNS servers supplied via the [[ref/object/dns-resolver#servers|`servers`]] field. Each entry is a `DnsCustomResolver` with its own [[ref/object/dns-resolver#dnscustomresolver|`address`]], [[ref/object/dns-resolver#dnscustomresolver|`port`]] (default `53`), and [[ref/object/dns-resolver#dnscustomresolver|`protocol`]] (`udp`, `tcp`, or `tls`).

DNSBL lookups are typically performed against the system resolver or a custom resolver, since most public DNSBL services do not accept queries from public open resolvers.

## Common settings

[Section titled “Common settings”](#common-settings)

Every variant exposes the following fields with identical semantics:

[[ref/object/dns-resolver#preserveintermediates|`preserveIntermediates`]]: whether to preserve intermediate name servers in resolution results. Default `true`.
[[ref/object/dns-resolver#concurrency|`concurrency`]]: maximum number of concurrent resolution requests. Default 2.
[[ref/object/dns-resolver#timeout|`timeout`]]: time after which a resolution request is considered failed. Default 5 seconds.
[[ref/object/dns-resolver#attempts|`attempts`]]: number of times a resolution request is retried before failing. Default 2.
[[ref/object/dns-resolver#tcponerror|`tcpOnError`]]: whether to retry over TCP when a UDP query errors. Default `true`.
[[ref/object/dns-resolver#enableedns|`enableEdns`]]: whether to enable EDNS, required for some DNS features such as DNSSEC. Default `true`.

A System-resolver configuration using the default values:

```
{  "@type": "System",  "preserveIntermediates": true,  "concurrency": 2,  "timeout": "5s",  "attempts": 2,  "tcpOnError": true,  "enableEdns": true}
```

A custom resolver pointing at two local DNS servers and one public one:

```
{  "@type": "Custom",  "servers": [    {"address": "192.0.2.1", "port": 55, "protocol": "udp"},    {"address": "192.0.2.3", "port": 55, "protocol": "tcp"},    {"address": "8.8.8.8"}  ]}
```