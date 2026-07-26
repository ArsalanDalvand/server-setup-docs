---
title: "Administrators"
source: "https://stalw.art/docs/auth/authorization/administrator"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/authorization/administrator"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Administrators"
---
# Administrators

      

Stalwart has no dedicated “administrator account” type in the directory. Administrative capabilities are granted by assigning specific [[auth/authorization/permissions|permissions]] to regular [[auth/principals/individual|individual accounts]]. This avoids handing out blanket administrative privileges and allows each administrator to hold only the permissions required for the tasks they perform.

Assigning permissions to ordinary accounts lets administrative responsibilities be distributed without compromising security. For example, one account may be granted the ability to manage domain settings and user accounts without being able to reconfigure system-wide services. This principle of least privilege reduces the risk of accidental or malicious changes to critical parts of the server.

Administrators can create one or more [[ref/object/role|Role]] objects that group administrative permissions together and assign those roles to the appropriate accounts. This provides consistent control over who can perform which administrative tasks.

For emergency access when the regular administrative accounts are unavailable, see the [[configuration/recovery-mode#recovery-administrator|recovery administrator]] mechanism. It is an environment-variable credential that is only honoured while the server is running in [[configuration/recovery-mode|recovery mode]] or during first-time [[configuration/bootstrap-mode|bootstrap]], and is intended to be set temporarily while an incident is resolved.

## Impersonation

[Section titled “Impersonation”](#impersonation)

There is no dedicated master-user configuration. Mailbox-wide access is instead granted by assigning the `impersonate` [[auth/authorization/permissions|permission]] to a regular account, typically through a [[ref/object/role|Role]]. Any account that holds this permission can sign in on behalf of another account by supplying a composite login of the form `<target>%<impersonator>` together with the impersonator’s password.

For example, when the impersonating account is `[email protected]` and the target mailbox belongs to `[email protected]`, signing in as `[email protected]%[email protected]` with the password of `[email protected]` grants access to `john`’s mailbox. The same convention works for single-label account names on deployments that do not use `user@domain` logins. Because this access is broad, the `impersonate` permission should be granted sparingly and only for legitimate administrative tasks such as maintenance or incident response.