---
title: "Overview"
source: "https://stalw.art/docs/encryption"
source_format: official-html-to-markdown
language: "en"
doc_slug: "encryption"
category: "stalwart"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Overview"
---
# Overview

      

Encryption at rest is a term used to describe the process of encrypting data while it is stored, or “at rest”, on persistent media such as hard drives, solid-state drives, or other forms of digital storage. The purpose of encryption at rest is to ensure that data stored on these media is not readable or accessible without the necessary decryption keys. The process usually involves taking clear-text data and converting it into cipher-text using an encryption algorithm and an encryption key. The key is a secret string of bits used by the encryption algorithm to scramble the data in a specific way. Without the key, or without a way to derive it, the scrambled data remains unintelligible and secure.

Stalwart applies a user-centric encryption strategy where each user’s plain-text messages are automatically encrypted using their provided [[encryption/pgp|OpenPGP]] (Pretty Good Privacy) public key or [[encryption/smime|S/MIME]] (Secure/Multipurpose Internet Mail Extensions) certificate before the messages are written to disk. Only the intended recipient, who holds the corresponding private key, can decrypt and access the message contents. As a result, system administrators with access to the server’s data cannot decrypt and read the messages. Even if unauthorized access occurs, the stored messages appear as unintelligible ciphertext without the corresponding private key. This design protects each user’s messages against external compromise as well as internal threats.

## Configuration

[Section titled “Configuration”](#configuration)

Server-wide encryption-at-rest behaviour is configured on the [[ref/object/email|Email]] singleton (found in the WebUI under  Settings ›  Email › Defaults,  Settings ›  Email › Encryption,  Settings ›  Email › Limits,  Settings ›  Email › Storage). Two fields govern the behaviour:

[[ref/object/email#encryptatrest|`encryptAtRest`]]: when `true` (the default), messages delivered via SMTP or LMTP are automatically encrypted before being written to disk, provided the recipient has registered an S/MIME certificate or OpenPGP public key. Setting this field to `false` disables the feature for all accounts.
[[ref/object/email#encryptonappend|`encryptOnAppend`]]: when `true`, messages manually appended by a client over JMAP or IMAP are also encrypted. The default is `false`, which leaves appended messages untouched so that clients retain full control over the contents they store.

The server-wide [[ref/object/email#encryptonappend|`encryptOnAppend`]] value is an override: when set to `true`, it applies to every account regardless of the per-account `encryptOnAppend` carried inside [[ref/object/account-settings#encryptionatrest|`encryptionAtRest`]]. When the server-wide flag is `false`, the per-account setting applies.

For example:

```
{  "encryptAtRest": true,  "encryptOnAppend": false}
```

Per-account encryption settings, including the selection of a specific public key and the symmetric algorithm, are carried on the [[ref/object/account-settings|AccountSettings]] singleton (found in the WebUI under  Account ›  Settings) through the [[ref/object/account-settings#encryptionatrest|`encryptionAtRest`]] field. Public keys themselves are stored as [[ref/object/public-key|PublicKey]] objects (found in the WebUI under  Account ›  Public Keys); see [[encryption/pgp|OpenPGP]] and [[encryption/smime|S/MIME]] for the accepted key formats, and [[encryption/manage|Management]] for how end users manage their own settings.