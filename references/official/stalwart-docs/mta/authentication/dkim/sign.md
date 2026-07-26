---
title: "Signing"
source: "https://stalw.art/docs/mta/authentication/dkim/sign"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/authentication/dkim/sign"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Signing"
---
# Signing

      

Stalwart can automatically sign outgoing messages with one or more DKIM signatures. The following algorithms are supported:

`ED25519-SHA256`: the Edwards-Curve Digital Signature Algorithm, known for strong security and efficient processing.
`RSA-SHA256`: RSA combined with SHA-256.
`RSA-SHA1`: supported for compatibility with legacy systems only; not recommended for new deployments.

When using RSA-based DKIM signatures, the key size should be at least 2048 bits. RSA keys smaller than 1024 bits are vulnerable to cryptographic attacks.

## Selecting signatures per message

[Section titled “Selecting signatures per message”](#selecting-signatures-per-message)

Whether to sign a given message (and with which signature(s)) is controlled by the [[ref/object/sender-auth#dkimsigndomain|`dkimSignDomain`]] field on the [[ref/object/sender-auth|SenderAuth]] singleton (found in the WebUI under  Settings ›  MTA › Inbound › Sender Authentication). The field accepts an expression that returns the domain whose DKIM signatures should be applied, or `false` to skip signing. The default policy returns the sender domain when the message is sent by an authenticated user from a local domain:

```
{  "dkimSignDomain": {    "match": [{"if": "is_local_domain(sender_domain) && !is_empty(authenticated_as)", "then": "sender_domain"}],    "else": "false"  }}
```

When a domain is returned, Stalwart selects every [[ref/object/dkim-signature|DkimSignature]] associated with that domain and signs the outgoing message with each of them. This allows, for example, an Ed25519 signature and an RSA signature to be applied simultaneously by creating two signatures against the same domain.

## Defining signatures

[Section titled “Defining signatures”](#defining-signatures)

DKIM signatures are [[ref/object/dkim-signature|DkimSignature]] objects (found in the WebUI under  Management ›  Domains › DKIM Signatures). Each instance is a multi-variant object: the `Dkim1Ed25519Sha256` variant covers Ed25519 signatures and the `Dkim1RsaSha256` variant covers RSA signatures. Both variants share the same configuration fields:

[[ref/object/dkim-signature#privatekey|`privateKey`]]: the PEM-encoded private key used for signing. Can be supplied directly or read from an environment variable or file.
[[ref/object/dkim-signature#domainid|`domainId`]]: the identifier of the [[ref/object/domain|Domain]] this signature belongs to.
[[ref/object/dkim-signature#selector|`selector`]]: the selector used to locate the public key in DNS.
[[ref/object/dkim-signature#headers|`headers`]]: the list of headers included in the signature. Defaults to `["From", "To", "Date", "Subject", "Message-ID"]`.
[[ref/object/dkim-signature#canonicalization|`canonicalization`]]: the canonicalization algorithm applied to the headers and body. Defaults to `relaxed/relaxed`.
[[ref/object/dkim-signature#expire|`expire`]]: optional signature lifetime.
[[ref/object/dkim-signature#thirdparty|`thirdParty`]]: optional authorised third-party signature value.
[[ref/object/dkim-signature#thirdpartyhash|`thirdPartyHash`]]: optional hashing algorithm (`sha256` or `sha1`) used to verify third-party signature DNS records.
[[ref/object/dkim-signature#auid|`auid`]]: optional Agent User Identifier included in the signature header.
[[ref/object/dkim-signature#report|`report`]]: whether to request failure reports when signature verification fails on the recipient side. Default `true`.
[[ref/object/dkim-signature#stage|`stage`]]: the rotation stage (`active`, `pending`, `retiring`, `retired`). Active keys sign outgoing mail; pending keys are staged for DNS publication; retiring keys have been superseded but still published in DNS; retired keys have been removed from DNS and are pending deletion.
[[ref/object/dkim-signature#nexttransitionat|`nextTransitionAt`]]: optional date when the key transitions to the next rotation stage.

For example, a pair of RSA and Ed25519 signatures for `example.org`, each covering an extended set of headers, looks like this:

```
[  {    "@type": "Dkim1RsaSha256",    "domainId": "<Domain id>",    "selector": "rsa-default",    "privateKey": {"@type": "File", "filePath": "/opt/stalwart/etc/private/dkim-rsa.key"},    "headers": ["From", "To", "Cc", "Date", "Subject", "Message-ID", "Organization", "MIME-Version", "Content-Type", "In-Reply-To", "References", "List-Id", "User-Agent", "Thread-Topic", "Thread-Index"],    "canonicalization": "relaxed/relaxed",    "expire": "10d",    "report": true,    "stage": "active"  },  {    "@type": "Dkim1Ed25519Sha256",    "domainId": "<Domain id>",    "selector": "ed-default",    "privateKey": {"@type": "File", "filePath": "/opt/stalwart/etc/private/dkim-ed.key"},    "headers": ["From", "To", "Cc", "Date", "Subject", "Message-ID", "Organization", "MIME-Version", "Content-Type", "In-Reply-To", "References", "List-Id", "User-Agent", "Thread-Topic", "Thread-Index"],    "canonicalization": "relaxed/relaxed",    "report": false,    "stage": "active"  }]
```

## Multiple domains

[Section titled “Multiple domains”](#multiple-domains)

Although a single DKIM key may be used across multiple domains with [Authorized Third-Party Signatures](https://www.ietf.org/rfc/rfc6541.html), a separate key per domain is recommended. This gives more granular control and makes issues easier to isolate. Because [[ref/object/sender-auth#dkimsigndomain|`dkimSignDomain`]] returns a domain name, the correct signatures are selected automatically as long as each domain has its own signature definitions.

## Generating keys

[Section titled “Generating keys”](#generating-keys)

The simplest way to generate and publish a DKIM key pair is from the [[management/webui|WebUI]]. Keys may also be generated manually using `openssl`.

### RSA

[Section titled “RSA”](#rsa)
Terminal window

```
openssl genrsa -out rsa_private.key 2048openssl rsa -in rsa_private.key -pubout -out rsa_public.key
```

These commands generate a 2048-bit RSA private key and derive the public key. The private key must be kept secure.

### Edwards-curve Digital Signature Algorithm

[Section titled “Edwards-curve Digital Signature Algorithm”](#edwards-curve-digital-signature-algorithm)
Terminal window

```
openssl genpkey -algorithm ed25519 -out ed_private.keyopenssl pkey -in ed_private.key -pubout -out ed_public.key
```

## Publishing DKIM keys

[Section titled “Publishing DKIM keys”](#publishing-dkim-keys)

Once the public key is available, it is published in DNS as a TXT record. The record name follows the form `selector._domainkey.yourdomain.org`.

To obtain an RSA DKIM public key in a format suitable for a DNS TXT record:

Terminal window

```
openssl rsa -in private.key -pubout -outform der 2>/dev/null | openssl base64 -A
```

For an Ed25519 DKIM public key:

Terminal window

```
openssl asn1parse -in ed_public.key -offset 12 -noout -out /dev/stdout | base64
```

The result is a base64-encoded public key that can be wrapped into a standard DKIM TXT record:

```
v=DKIM1; k=rsa; p=PUBLIC_KEY
```

For example, with domain `example.org`, selector `default`, and an RSA public key `ABCDEFG`:

```
default._domainkey.example.org. IN TXT "v=DKIM1; k=rsa; p=ABCDEFG"
```

The signature must use the matching selector and domain.