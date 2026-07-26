---
title: "Overview"
source: "https://stalw.art/docs/mta/rewrite"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/rewrite"
category: "mta"
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

      

Message rewriting alters aspects of an email message as it is processed by the mail server. This can involve changing the sender or recipient addresses in the envelope (the ‘outside’ of the message), or modifying the content itself, including headers, body, and attachments.

Typical reasons for rewriting a message include [[mta/rewrite/address|changing recipient addresses]] to redirect mail traffic, obfuscating the original sender’s address for privacy, or altering the domain in an address to match an organisation’s branding. Modifying the content of a message is useful for managing how it is presented to the recipient: operators may [[mta/rewrite/headers|add, delete, or change headers]] for tracking or compliance, [[mta/rewrite/content|alter the body]] to append a disclaimer or signature, or manage attachments for security, for example by removing potentially harmful files.

Stalwart supports both address rewriting and content modification.