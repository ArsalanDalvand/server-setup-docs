---
title: "ARC"
source: "https://stalw.art/docs/mta/authentication/arc"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/authentication/arc"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ARC"
---
# ARC

      

Authenticated Received Chain (ARC) is an email-authentication protocol that establishes a chain of trust between email domains. It allows receivers to authenticate the original sender of a message even after it has been forwarded multiple times. Each hop in the delivery path appends a set of `ARC-` headers capturing its own authentication results, and downstream verifiers can evaluate the whole chain to decide whether to trust a message whose SPF or DKIM might otherwise have been invalidated by forwarding.

## Sealing

[Section titled “Sealing”](#sealing)

ARC sealing is the process by which a receiving mail server adds its own digital signature to the existing ARC chain, allowing subsequent recipient servers to verify the chain. Following the consensus reached in the DMARC IETF Working Group to wind down the ARC experiment, Stalwart does not seal messages. Only [verification](#verification) of existing ARC chains remains supported, for compatibility with systems that still produce ARC sets.

## Verification

[Section titled “Verification”](#verification)

Stalwart can verify the ARC seals and signatures of incoming messages using the `ED25519-SHA256`, `RSA-SHA256`, and `RSA-SHA1` algorithms. ARC verification is configured on the [[ref/object/sender-auth|SenderAuth]] singleton (found in the WebUI under  Settings ›  MTA › Inbound › Sender Authentication). The [[ref/object/sender-auth#arcverify|`arcVerify`]] field accepts an expression that returns one of:

`relaxed`: verify ARC and report the results in the `Authentication-Results` header.
`strict`: reject the message if all ARC seals fail verification; otherwise report the results.
`disable`: do not perform ARC verification.

By default `arcVerify` resolves to `disable`, so ARC verification must be explicitly enabled. To verify ARC in relaxed mode for every incoming message:

```
{  "arcVerify": {"else": "relaxed"}}
```