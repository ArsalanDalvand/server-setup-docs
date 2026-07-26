---
title: "Proxy Protocol"
source: "https://stalw.art/docs/server/reverse-proxy/proxy-protocol"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/reverse-proxy/proxy-protocol"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Proxy Protocol"
---
# Proxy Protocol

      

The Proxy Protocol (also known as the HAProxy protocol) extends a TCP connection with metadata describing the original client. Without it, a server behind a proxy sees only the proxy’s IP address, which breaks IP-based access controls, sender authentication, and accurate logging. The Proxy Protocol prepends each forwarded connection with a small header that the back-end server parses to recover the original client IP and the TLS status of the original connection.

When Stalwart runs behind HAProxy, Caddy, NGINX, or Traefik, enabling the Proxy Protocol preserves every signal the server needs. The client’s remote IP is required for SPF and DMARC checks; per-IP rate limiting and auto-banning need the real source address; and whether the incoming connection was protected by TLS is a policy input on several code paths.

## Configuration

[Section titled “Configuration”](#configuration)

Stalwart supports both version 1 and version 2 of the Proxy Protocol, including the version-2 TLV extensions. The Proxy Protocol is enabled by declaring the set of trusted IP addresses or CIDR ranges from which Proxy Protocol headers will be accepted. Connections from other addresses are rejected if they carry a Proxy Protocol header and processed without it otherwise.

Server-wide trusted networks are set through [[ref/object/system-settings#proxytrustednetworks|`proxyTrustedNetworks`]] on the [[ref/object/system-settings|SystemSettings]] singleton (found in the WebUI under  Settings ›  Network › Services,  Settings ›  Network › General). Each entry is an IP address or a CIDR mask, for example `127.0.0.0/8`, `::1`, or `10.0.0.0/8`.

The same list can also be set on an individual [[ref/object/network-listener|NetworkListener]] through [[ref/object/network-listener#overrideproxytrustednetworks|`overrideProxyTrustedNetworks`]]. The per-listener value replaces the server-wide list for that listener, so different listeners can trust different proxies.