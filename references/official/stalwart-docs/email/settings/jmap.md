---
title: "JMAP for Mail"
source: "https://stalw.art/docs/email/settings/jmap"
source_format: official-html-to-markdown
language: "en"
doc_slug: "email/settings/jmap"
category: "email"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "JMAP for Mail"
---
# JMAP for Mail

      

JMAP for Mail ([RFC8621](https://www.rfc-editor.org/rfc/rfc8621.html)) is an extension to the [JMAP Core protocol](https://www.rfc-editor.org/rfc/rfc8620.html) that allows users to manage their email accounts using the JMAP API. The extension covers uploading, deleting, and listing emails, as well as managing mailboxes and related settings. Message-level limits and mailbox-shape constraints are carried on the [[ref/object/email|Email]] singleton (found in the WebUI under  Settings ›  Email › Defaults,  Settings ›  Email › Encryption,  Settings ›  Email › Limits,  Settings ›  Email › Storage); JMAP request-parsing limits are carried on the [[ref/object/jmap|Jmap]] singleton (found in the WebUI under  Settings ›  Network › JMAP › Limits,  Settings ›  Network › JMAP › Push,  Settings ›  Network › JMAP › WebSocket).

## Mailbox settings

[Section titled “Mailbox settings”](#mailbox-settings)

Mailbox shape is controlled through two fields on the Email singleton:

[[ref/object/email#maxmailboxdepth|`maxMailboxDepth`]]: maximum depth of nested mailboxes a user can create. Default `10`.
[[ref/object/email#maxmailboxnamelength|`maxMailboxNameLength`]]: maximum length of a mailbox name. Default `255`.

Example:

```
{  "maxMailboxDepth": 10,  "maxMailboxNameLength": 255}
```

## Email settings

[Section titled “Email settings”](#email-settings)

Message-size limits are set on the Email singleton:

[[ref/object/email#maxattachmentsize|`maxAttachmentSize`]]: maximum size, in bytes, for an email attachment. Default `50000000`.
[[ref/object/email#maxmessagesize|`maxMessageSize`]]: maximum size, in bytes, for an email message. Default `75000000`.

The upper bound on the number of email messages that can be parsed in a single JMAP request is set on the Jmap singleton through [[ref/object/jmap#parselimitemail|`parseLimitEmail`]], which defaults to `10`.

Example:

```
{  "maxAttachmentSize": 50000000,  "maxMessageSize": 75000000}
```

And on the Jmap singleton:

```
{  "parseLimitEmail": 10}
```