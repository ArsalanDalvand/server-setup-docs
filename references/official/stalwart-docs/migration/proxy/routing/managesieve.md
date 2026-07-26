---
title: "ManageSieve"
source: "https://stalw.art/docs/migration/proxy/routing/managesieve"
source_format: official-html-to-markdown
language: "en"
doc_slug: "migration/proxy/routing/managesieve"
category: "migration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ManageSieve"
---
# ManageSieve

      

The ManageSieve listener lets clients manage Sieve scripts on the resolved backend. It is typically served with STARTTLS on port 4190. As with the other interactive protocols, the proxy answers the pre-authentication dialogue itself and bridges the session once the backend accepts the replayed authentication.

## Pre-authentication

[Section titled “Pre-authentication”](#pre-authentication)

ManageSieve begins with the server announcing its capabilities, so on connection the proxy sends a capability block followed by an `OK` line. The block is built from `[capabilities.managesieve]` and reports the implementation string, the protocol version, the advertised SASL mechanisms, the supported Sieve extensions from the `sieve` list, and, on a STARTTLS listener before the upgrade, `STARTTLS`. Before authentication the proxy handles `CAPABILITY` (re-sending the block), `NOOP`, `LOGOUT`, and `STARTTLS`.

## Authentication

[Section titled “Authentication”](#authentication)

Authentication uses the `AUTHENTICATE` command with a SASL mechanism, one of `PLAIN`, `OAUTHBEARER` or `XOAUTH2`. The SASL initial response may be supplied inline or as a literal after the proxy’s continuation prompt, following the ManageSieve literal syntax. Cleartext `PLAIN` authentication is gated on transport security and refused until the connection is encrypted unless `allow_plain_auth_without_tls` is set. The routing identifier is extracted from the credentials as described under [[migration/proxy/routing|Routing]].

## Backend handshake

[Section titled “Backend handshake”](#backend-handshake)

The proxy connects to the destination’s ManageSieve endpoint and reads the backend’s capability block. On a STARTTLS endpoint it confirms the backend advertises `STARTTLS`, issues the command, upgrades the connection, and reads the refreshed capabilities. It then verifies the backend leg is encrypted.

When the destination uses `XCLIENT` forwarding and the backend advertised `XCLIENT`, the proxy issues an `XCLIENT` command carrying the client and destination addresses, a session identifier and the transport security before authenticating. It then replays the client’s `AUTHENTICATE` command, relays the result, and bridges the session on success.