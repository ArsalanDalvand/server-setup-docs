---
title: "Management"
source: "https://stalw.art/docs/encryption/manage"
source_format: official-html-to-markdown
language: "en"
doc_slug: "encryption/manage"
category: "encryption"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Management"
---
# Management

      

Stalwart exposes encryption-at-rest settings through the [[management/webui/account-manager|self-service portal]], which allows end users to register their own keys and toggle encryption for their accounts without involving a system administrator. The same settings are reachable over the JMAP API (through the [[ref/object/account-settings|AccountSettings]] and [[ref/object/public-key|PublicKey]] objects) and through the [[management/cli|CLI]].

Each account may register one or more [[ref/object/public-key|PublicKey]] objects holding an OpenPGP public key or an S/MIME certificate. Encryption at rest is then activated by referencing one of these keys from the [[ref/object/account-settings#encryptionatrest|`encryptionAtRest`]] field on the [[ref/object/account-settings|AccountSettings]] singleton.

## Enabling encryption

[Section titled “Enabling encryption”](#enabling-encryption)

End users enable encryption from the self-service portal by performing two steps:

Register at least one public key as a [[ref/object/public-key|PublicKey]] object. Upload the OpenPGP public key or S/MIME certificate and fill in [[ref/object/public-key#description|`description`]]; optional fields include [[ref/object/public-key#expiresat|`expiresAt`]] and [[ref/object/public-key#emailaddresses|`emailAddresses`]]. The key material goes into [[ref/object/public-key#key|`key`]].
Update the [[ref/object/account-settings|AccountSettings]] singleton and set the [[ref/object/account-settings#encryptionatrest|`encryptionAtRest`]] field to the chosen algorithm variant, referencing the registered key by id.

The [[ref/object/account-settings#encryptionatrest|`encryptionAtRest`]] field accepts three variants: `Disabled`, `Aes128`, and `Aes256`. The `Aes128` and `Aes256` variants carry a [[ref/object/account-settings#encryptionsettings|`publicKey`]] identifier, an [[ref/object/account-settings#encryptionsettings|`encryptOnAppend`]] boolean controlling whether messages appended over JMAP or IMAP are also encrypted, and an [[ref/object/account-settings#encryptionsettings|`allowSpamTraining`]] boolean controlling whether the spam classifier is trained on the plaintext before encryption.

For example, enabling AES-256 encryption with a registered key:

```
{  "encryptionAtRest": {    "@type": "Aes256",    "publicKey": "id1",    "encryptOnAppend": false,    "allowSpamTraining": false  }}
```

After registering a key and selecting an algorithm, send a test message to the account. The message should arrive encrypted, and the mail client must hold the corresponding private key to decrypt it. If decryption fails, verify that the client is configured with the matching private key for the OpenPGP public key, or the S/MIME certificate private key, that was uploaded to the server.

## Disabling encryption

[Section titled “Disabling encryption”](#disabling-encryption)

To disable encryption at rest for an account, update the [[ref/object/account-settings|AccountSettings]] singleton and set [[ref/object/account-settings#encryptionatrest|`encryptionAtRest`]] to the `Disabled` variant:

```
{  "encryptionAtRest": {    "@type": "Disabled"  }}
```

New messages are then stored in plain text. Messages encrypted while the feature was enabled remain encrypted on disk; disabling the setting does not decrypt existing content. Re-enabling encryption at a later point applies only to messages received afterwards and does not retroactively encrypt previously delivered messages.