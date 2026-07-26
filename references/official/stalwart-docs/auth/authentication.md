---
title: "Overview"
source: "https://stalw.art/docs/auth/authentication"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/authentication"
category: "auth"
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

      

Stalwart authenticates users against several [[auth/backend|backends]], allowing integration with existing identity systems. Supported backends include LDAP directories, SQL databases such as PostgreSQL, MySQL, and SQLite, and an internal directory managed directly by the server.

Stalwart also supports [[auth/openid|OpenID Connect]] and [[auth/oauth|OAuth]], which allow authentication against the built-in OpenID server or integration with external identity providers. Security controls include authentication rate limiting to reduce the risk of password guessing, and built-in [[server/auto-ban|Fail2Ban]] support, which applies temporary IP bans to sources that repeatedly trigger suspicious activity.