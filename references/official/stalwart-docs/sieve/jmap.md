---
title: "JMAP for Sieve"
source: "https://stalw.art/docs/sieve/jmap"
source_format: official-html-to-markdown
language: "en"
doc_slug: "sieve/jmap"
category: "sieve"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "JMAP for Sieve"
---
# JMAP for Sieve

      

JMAP for Sieve ([RFC9661](https://www.rfc-editor.org/rfc/rfc9661.html)) is an extension to the [[http/jmap|JMAP protocol]] that allows users to manage their Sieve scripts using the JMAP API. This extension provides a way for users to upload, delete, and list their Sieve scripts, as well as to set a default script for their account.
Support for JMAP for Sieve is enabled by default in Stalwart, and no additional configuration is required. However, it is possible to disable on a per-user basis by disabling the JMAP for Sieve [[auth/authorization/permissions|permissions]] for the user.