---
title: "App Passwords"
source: "https://stalw.art/docs/auth/authentication/app-password"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/authentication/app-password"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "App Passwords"
---
# App Passwords

      

Application Passwords are unique passwords that allow users to access their email accounts from devices or applications that do not support [[auth/authentication/2fa|Two-Factor Authentication]]. They provide a way to use legacy mail clients or tools that do not support the `OAUTHBEARER` or `XOAUTH2` SASL mechanisms while preserving the benefits of 2FA on the primary account password.

Application Passwords are most useful in a few scenarios. Older email clients that do not support OAuth cannot prompt for a TOTP code, so standard 2FA is impractical with them; an Application Password allows these clients to authenticate securely. Third-party applications and services that have not adopted modern authentication mechanisms can also connect using an Application Password rather than the main account password. Finally, automated scripts and tools that need non-interactive access to a mailbox can authenticate with an Application Password scoped to the required permissions.

Each Application Password is managed as a distinct credential, so it can be named, inspected, and revoked without affecting other sessions.

## Managing App Passwords

[Section titled “Managing App Passwords”](#managing-app-passwords)

Users create and remove Application Passwords from the [[management/webui|self-service portal]], under the App Passwords menu option. The portal lists existing Application Passwords and allows individual entries to be revoked.

Each Application Password is represented by an [[ref/object/app-password|AppPassword]] object (found in the WebUI under  Account ›  Credentials › App Passwords). The credential carries a [[ref/object/app-password#description|`description`]], an optional [[ref/object/app-password#expiresat|`expiresAt`]], an optional list of [[ref/object/app-password#allowedips|`allowedIps`]] that restrict where the credential may be used, and a [[ref/object/app-password#permissions|`permissions`]] mode controlling whether the credential inherits, restricts, or replaces the account’s permissions. The secret itself is server-set and returned only on creation.

Administrators have limited control over Application Passwords. They can view and revoke a user’s Application Passwords but cannot create new ones on a user’s behalf.

## Internal Storage

[Section titled “Internal Storage”](#internal-storage)

Each Application Password is stored on the account as one of its secrets, in the form `$app$name$password`, where `$app$` marks the secret as an Application Password, `name` is the unique identifier for the credential, and `password` is the hashed secret. The `$app$` prefix keeps Application Passwords distinguishable from other credential kinds, and storing only the hash of the generated secret prevents the raw password from being recovered from the directory.