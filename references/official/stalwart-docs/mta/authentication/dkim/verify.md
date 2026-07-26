---
title: "Verifying"
source: "https://stalw.art/docs/mta/authentication/dkim/verify"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/authentication/dkim/verify"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Verifying"
---
# Verifying

      

Stalwart can verify the DKIM signatures of incoming messages using the `ED25519-SHA256`, `RSA-SHA256`, or `RSA-SHA1` algorithms. Verification is configured on the [[ref/object/sender-auth|SenderAuth]] singleton (found in the WebUI under  Settings ›  MTA › Inbound › Sender Authentication).

The [[ref/object/sender-auth#dkimverify|`dkimVerify`]] field accepts an expression that returns one of:

`relaxed`: verify DKIM and report the results in the `Authentication-Results` header.
`strict`: reject the message if all DKIM signatures fail verification; otherwise report the results.
`disable`: do not perform DKIM verification.

The default is `relaxed`:

```
{  "dkimVerify": {"else": "relaxed"}}
```

## Insecure signatures

[Section titled “Insecure signatures”](#insecure-signatures)

An insecure DKIM signature is one whose parameters, although conforming to the standards, can be exploited. A common example is the DKIM `l=` parameter, which specifies the exact number of octets in the message body covered by the signature. An attacker can append content beyond the covered length, and the original signature remains valid for the covered portion. Such signatures can mislead automated systems and end-users, undermining trust indicators such as Brand Indicators for Message Identification (BIMI).

Stalwart ignores insecure signatures by default. To accept all signatures, including those deemed insecure, set [[ref/object/sender-auth#dkimstrict|`dkimStrict`]] to `false`:

```
{  "dkimStrict": false}
```