---
title: "Reverse IP"
source: "https://stalw.art/docs/mta/authentication/iprev"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/authentication/iprev"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Reverse IP"
---
# Reverse IP

      

Reverse IP verification validates the authenticity of the connecting client’s IP address. The server performs a reverse DNS lookup of the remote IP and compares the result against the hostname provided in the `EHLO` or `HELO` command. If they do not match, the server can reject the connection to guard against hosts that disguise their origin when sending spam or performing other malicious activity. Reverse-IP verification helps ensure that incoming SMTP connections are legitimate.

## Settings

[Section titled “Settings”](#settings)

Reverse IP verification is configured on the [[ref/object/sender-auth|SenderAuth]] singleton (found in the WebUI under  Settings ›  MTA › Inbound › Sender Authentication). The [[ref/object/sender-auth#reverseipverify|`reverseIpVerify`]] field accepts an expression that returns one of:

`relaxed`: verify the reverse IP and report the results in the `Authentication-Results` header.
`strict`: reject the message if reverse-IP verification fails; otherwise report the results.
`disable`: do not perform reverse-IP verification.

The default policy applies `relaxed` when `local_port == 25` and `disable` otherwise, equivalent to:

```
{  "reverseIpVerify": {    "match": [{"if": "listener == 'smtp'", "then": "relaxed"}],    "else": "disable"  }}
```