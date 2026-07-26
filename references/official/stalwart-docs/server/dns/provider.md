---
title: "DNS Providers"
source: "https://stalw.art/docs/server/dns/provider"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/dns/provider"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DNS Providers"
---
# DNS Providers

      

The [[ref/object/dns-server|DnsServer]] object (found in the WebUI under  Settings ›  Network › DNS › DNS Providers) is multi-variant, and each variant corresponds to a supported provider or protocol. Each variant carries its own credential fields (API keys, tokens, service-account JSON, or account identifiers as appropriate) alongside the shared timing fields that control TTLs, polling, and propagation checks. The authoritative list of fields for every variant is documented on the [[ref/object/dns-server|DnsServer]] reference page.

## Self-hosted BIND-compatible servers

[Section titled “Self-hosted BIND-compatible servers”](#self-hosted-bind-compatible-servers)

Two variants implement dynamic DNS updates as defined in RFC 2136 for self-hosted authoritative name servers:

The `Tsig` variant authenticates updates with a shared TSIG key.
The `Sig0` variant authenticates updates with a SIG(0) asymmetric key pair.

## Hosted DNS APIs

[Section titled “Hosted DNS APIs”](#hosted-dns-apis)

The remaining variants integrate with hosted DNS services through each provider’s management API:

Cloudflare
AWS Route 53
Google Cloud DNS
OVH
deSEC
DigitalOcean
Bunny DNS
Porkbun
DNSimple
Spaceship