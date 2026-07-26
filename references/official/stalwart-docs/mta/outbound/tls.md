---
title: "TLS"
source: "https://stalw.art/docs/mta/outbound/tls"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/outbound/tls"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "TLS"
---
# TLS

      

Stalwart uses TLS to secure message transmission over the internet. TLS (Transport Layer Security) provides encryption, authentication, and integrity for data in transit, preventing unauthorised access and tampering. Stalwart’s TLS implementation is built on rustls, a high-performance TLS library.

A TLS strategy defines the transport-security policies and TLS settings that apply when Stalwart connects to remote mail servers. Where connection strategies control the basic mechanics of establishing a session, TLS strategies determine the security requirements for that session.

Each TLS strategy allows control over DANE enforcement, MTA-STS enforcement, STARTTLS handling, certificate validation, and TLS timeouts. TLS strategies are defined as [[ref/object/mta-tls-strategy|MtaTlsStrategy]] objects (found in the WebUI under  Settings ›  MTA › Outbound › TLS Strategies), and selected dynamically for each delivery via the TLS expression on [[ref/object/mta-outbound-strategy|MtaOutboundStrategy]]; see [[mta/outbound/strategy|Strategies]].

## Policy enforcement

[Section titled “Policy enforcement”](#policy-enforcement)

Transport-security policies on outbound connections are governed by three fields, each of which accepts one of `optional`, `require`, or `disable`:

`optional`: use the mechanism when available, but do not require it.
`require`: enforce the mechanism strictly; delivery fails if it cannot be used.
`disable`: do not attempt to use the mechanism.

### DANE

[Section titled “DANE”](#dane)

[[mta/transport-security/dane|DANE]] (DNS-Based Authentication of Named Entities) allows SMTP clients to validate a recipient server’s TLS certificate using DNSSEC-protected TLSA records. The [[ref/object/mta-tls-strategy#dane|`dane`]] field controls enforcement. When set to `require`, delivery only proceeds if the recipient publishes valid and verifiable TLSA records. When set to `optional` (the default), DANE validation is attempted when possible and the connection falls back to regular STARTTLS when no TLSA record is available.

```
{  "name": "secure",  "dane": "require"}
```

### MTA-STS

[Section titled “MTA-STS”](#mta-sts)

[[mta/transport-security/mta-sts|MTA-STS]] (Mail Transfer Agent Strict Transport Security) allows recipient domains to publish a required TLS policy over HTTPS. The [[ref/object/mta-tls-strategy#mtasts|`mtaSts`]] field controls enforcement. When set to `require`, Stalwart enforces the domain’s published policy and refuses delivery if a secure connection cannot be negotiated. When set to `optional` (the default), the policy is used when it can be retrieved but delivery proceeds even if it cannot be validated.

```
{  "name": "default",  "mtaSts": "optional"}
```

### STARTTLS

[Section titled “STARTTLS”](#starttls)

STARTTLS upgrades a plaintext SMTP connection to a secure TLS connection. The [[ref/object/mta-tls-strategy#starttls|`startTls`]] field controls its use. When set to `require`, delivery only proceeds if the remote server supports STARTTLS and the handshake succeeds. When set to `disable`, STARTTLS is never attempted and delivery occurs over plaintext. The default is `optional`.

```
{  "name": "legacy",  "startTls": "disable"}
```

## Invalid certificates

[Section titled “Invalid certificates”](#invalid-certificates)

The [[ref/object/mta-tls-strategy#allowinvalidcerts|`allowInvalidCerts`]] field controls how Stalwart handles invalid or misconfigured TLS certificates from remote hosts. When set to `false` (the default), connections to remote hosts that present an invalid TLS certificate are rejected, ensuring that communications are authenticated and trustworthy.

Some remote hosts have certificates that do not match their hostname or are otherwise invalid. Setting `allowInvalidCerts` to `true` lets Stalwart connect anyway, but the security implications must be weighed: accepting invalid certificates opens the connection to man-in-the-middle attacks. The recommended setting is `false` unless there is a specific, monitored reason to relax it.

## Timeouts

[Section titled “Timeouts”](#timeouts)

Timeouts for TLS-related operations are governed by [[ref/object/mta-tls-strategy#tlstimeout|`tlsTimeout`]] (maximum time to wait for the TLS handshake to complete, default 3 minutes) and [[ref/object/mta-tls-strategy#mtaststimeout|`mtaStsTimeout`]] (maximum time to wait for the MTA-STS policy lookup, default 5 minutes):

```
{  "tlsTimeout": "3m",  "mtaStsTimeout": "3m"}
```

## Examples

[Section titled “Examples”](#examples)

### Handling TLS errors

[Section titled “Handling TLS errors”](#handling-tls-errors)

Stalwart can dynamically adjust TLS settings based on conditions encountered during outbound delivery. This is useful when dealing with remote hosts that have outdated or misconfigured TLS setups.

Because rustls deliberately excludes broken or obsolete protocols (SSL1/2/3, TLS 1.0/1.1) and cipher suites (RC4, DES, 3DES, EXPORT ciphers, MAC-then-encrypt, suites without forward secrecy, renegotiation, Kerberos, compression, discrete-log Diffie-Hellman, automatic protocol downgrades, AES-GCM with unsafe nonces), handshakes with poorly configured servers may fail. A common pattern is to define three MtaTlsStrategy objects, named for instance `default`, `invalid-tls`, and `disable-tls`, with progressively more relaxed settings, and to select between them through the TLS expression on [[ref/object/mta-outbound-strategy|MtaOutboundStrategy]]. The expression branches on `retry_num` and `last_error == 'tls'` so that a first retry uses the `invalid-tls` strategy, a second retry uses `disable-tls`, and all other cases use the `default` strategy.

The TLS expression on MtaOutboundStrategy:

```
{  "tls": {    "match": [      {"if": "retry_num > 0 && last_error == 'tls'", "then": "'invalid-tls'"},      {"if": "retry_num > 1 && last_error == 'tls'", "then": "'disable-tls'"}    ],    "else": "'default'"  }}
```

With the three paired MtaTlsStrategy objects:

```
[  {    "name": "invalid-tls",    "allowInvalidCerts": true,    "startTls": "optional"  },  {    "name": "disable-tls",    "allowInvalidCerts": false,    "startTls": "disable"  },  {    "name": "default",    "allowInvalidCerts": false,    "startTls": "optional"  }]
```

This approach balances secure transmission against the reality of occasionally misconfigured servers. Operators should monitor how often the relaxed strategies are selected and treat the default as the primary policy.

### Strict transport security

[Section titled “Strict transport security”](#strict-transport-security)

TLS strategies can also enforce strict transport security for specific hosts. A dedicated `high-security` MtaTlsStrategy might set `allowInvalidCerts = false`, `dane = require`, `mtaSts = require`, and `startTls = require`. The TLS expression on MtaOutboundStrategy can branch on `mx == 'highly-secure.host.org'` to select this strategy and fall back to `default` otherwise. With this configuration, messages to the target host are delivered only over an authenticated and encrypted connection.

```
{  "tls": {    "match": [{"if": "mx == 'highly-secure.host.org'", "then": "'high-security'"}],    "else": "'default'"  }}
```

Paired with two MtaTlsStrategy objects:

```
[  {    "name": "high-security",    "allowInvalidCerts": false,    "dane": "require",    "mtaSts": "require",    "startTls": "require"  },  {    "name": "default",    "allowInvalidCerts": false,    "startTls": "optional",    "mtaSts": "optional",    "dane": "optional"  }]
```