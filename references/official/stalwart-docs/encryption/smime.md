---
title: "S/MIME"
source: "https://stalw.art/docs/encryption/smime"
source_format: official-html-to-markdown
language: "en"
doc_slug: "encryption/smime"
category: "encryption"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "S/MIME"
---
# S/MIME

      

S/MIME (Secure/Multipurpose Internet Mail Extensions) is a widely accepted protocol for sending digitally signed or encrypted messages. It is built into most modern mail software and interoperates between implementations, which makes it one of the most common ways to send secure email.

S/MIME relies on public-key cryptography. Each user has a pair of keys: a public key, distributed freely and used by senders to encrypt the message, and a private key, kept secret by the user and used to decrypt received messages. To use S/MIME, users must obtain a digital certificate from a trusted certificate authority (CA). The certificate carries the user’s public key and identifies the user. The private key is kept securely by the user and is never shared.

Stalwart supports S/MIME and can encrypt messages using either AES-256-CBC or AES-128-CBC symmetric encryption. AES-256-CBC provides a higher security margin at the cost of more computational work per message; AES-128-CBC uses fewer resources while still providing a strong level of security.

## Obtaining a key pair

[Section titled “Obtaining a key pair”](#obtaining-a-key-pair)

S/MIME key pairs are typically obtained by requesting a certificate from a trusted Certificate Authority (CA). Certificates are usually distributed as a PKCS #12 file (also known as a PFX file), which contains the private key paired with the public key (the S/MIME certificate). These files are password-protected so the private key remains secure.

Once the PFX file is available, it should be imported into the user’s mail client. Each mail client has a slightly different import procedure, so the client-specific instructions should be followed. Once the S/MIME certificate is installed, the public certificate can be exported (without the private key) and shared with correspondents so they can send encrypted mail.

The private key from the S/MIME certificate must always remain private and secure. Any party that gains access to the private key can impersonate the account and decrypt its messages.

## Exporting certificates

[Section titled “Exporting certificates”](#exporting-certificates)

### Using a mail client

[Section titled “Using a mail client”](#using-a-mail-client)

S/MIME public certificates, which correspondents need to send encrypted mail, can be exported from most mail clients. The exact steps vary; refer to the client’s documentation. From the client’s settings, the installed certificates can usually be viewed and exported to a file.

### Using macOS

[Section titled “Using macOS”](#using-macos)

If the mail client does not offer a convenient export path, macOS users can use Keychain Access, the system’s password- and credential-management tool. Keychain Access stores certificates and keys alongside passwords; the S/MIME certificate can be located in the list of keys and exported directly.

### Using OpenSSL

[Section titled “Using OpenSSL”](#using-openssl)

OpenSSL can also export the public certificate from a PFX file. This approach requires familiarity with the command line. A typical command is:

Terminal window

```
$ openssl pkcs12 -in path_to_pfx_file -clcerts -nokeys -out public_cert.pem
```

For legacy PKCS#12 files that fail to load, the `-legacy` flag is available:

Terminal window

```
$ openssl pkcs12 -legacy -in path_to_pfx_file -clcerts -nokeys -out public_cert.pem
```

This produces a PEM-formatted public certificate at `public_cert.pem`. Replace `path_to_pfx_file` and `public_cert.pem` with the actual paths and filenames.

## Importing certificates

[Section titled “Importing certificates”](#importing-certificates)

A registered certificate is represented as a [[ref/object/public-key|PublicKey]] object (found in the WebUI under  Account ›  Public Keys). An account may register a single certificate or several; registering multiple certificates is useful during key rotation, allowing a switch to a new certificate while still being able to decrypt messages encrypted with the old one.

Stalwart accepts S/MIME certificates in DER and PEM formats. DER is a binary encoding; PEM is a Base64 encoding of the same data, typically wrapped between `-----BEGIN CERTIFICATE-----` and `-----END CERTIFICATE-----` lines. A single certificate can be supplied in either format; for a bundle of multiple certificates, PEM must be used.

For example:

```
-----BEGIN CERTIFICATE-----MIIDbjCCAlagAwIBAgIUZ4K0WXNSS8H0cUcZavD9EYqqTAswDQYJKoZIhvcNAQENBQAwLTErMCkGA1UEAxMiU2FtcGxlIExBTVBTIENlcnRpZmljYXRlIEF1dGhvcml0eTAgFw0xOTExMjAwNjU0MThaGA8yMDUyMDkyNzA2NTQxOFowGTEXMBUGA1UEAxMOQWxpY2UgTG92ZWxhY2UwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDD7q35ZdG2JAzzJGNZDZ9sV7AKh0hlRfoFjTZN5m4RegQAYSyag43ouWi1xRN0avf0UTYrwjK04qRdV7GzCACoEKq/xiNUOsjfJXzbCublN3fZMOXDshKKBqThlK75SjA9Czxg7ejGoiY/iidk0e91neK30SCCaBTJlfR2ZDrPk73IPMeksxoTatfF9hw9dDA+/Hi1yptN/aG0Q/s9icFrxr6y2zQXsjuQPmjMZgj10aD9cazWVgRYCgflhmA0V1uQl1wobYU8DAVxVn+GgabqyjGQMoythIK0Gn5+ofwxXXUM/zbU+g6+1ISdoXxRRFtq2GzbIqkAHZZQm+BbnFrhAgMBAAGjgZcwgZQwDAYDVR0TAQH/BAIwADAeBgNVHREEFzAVgRNhbGljZUBzbWltZS5leGFtcGxlMBMGA1UdJQQMMAoGCCsGAQUFBwMEMA8GA1UdDwEB/wQFAwMHoAAwHQYDVR0OBBYEFKwuVFqk/VUYry7oZkQ40SXR1wB5MB8GA1UdIwQYMBaAFLdSTXPAiD2yw3paDPOU9/eAonfbMA0GCSqGSIb3DQEBDQUAA4IBAQB76o4Yz7yrVSFcpXqLrcGtdI4q93aKCXECCCzNQLp4yesh6brqaZHNJtwYcJ5TqbUym9hJ70iJE4jGNN+yAZR1ltte0HFKYIBKM4EJumG++2hqbUaLz4tl06BHaQPCv/9NiNY7q9R9c/B6s1YzHhwqkWht2a+AtgJ4BkpG+g+MmZMQV/Ao7RwLFKJ9OlMWLBmEXFcpIJN0HpPasT0nEl/MmotSu+8RnClAi3yFfyTKb+8rD7VxuyXetqDZ6dU/9/iqD/SZS7OQIjywtd343mACz3B1RlFxMHSA6dQAf2btGumqR0KiAp3KkYRAePoaJqYkB7Zad06ngFl0G0FHON+7-----END CERTIFICATE-----
```

The certificate data is carried in the [[ref/object/public-key#key|`key`]] field of the PublicKey object, alongside a required [[ref/object/public-key#description|`description`]] and optional [[ref/object/public-key#expiresat|`expiresAt`]] and [[ref/object/public-key#emailaddresses|`emailAddresses`]] fields. Registered certificates are then referenced from [[ref/object/account-settings|AccountSettings]]; see the [[encryption/manage|management interface]] for how to select a certificate and enable encryption.