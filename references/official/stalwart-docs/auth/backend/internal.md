---
title: "Internal"
source: "https://stalw.art/docs/auth/backend/internal"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/backend/internal"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Internal"
---
# Internal

      

The internal directory handles credential validation, email address lookup, and storage of account-level settings such as disk quotas and group memberships. It is suitable for deployments where Stalwart is the primary identity store and no external directory is in use. All account management, including the creation of new accounts, password changes, and quota adjustments, is carried out directly on the server.

Internally, account records are kept in the configured [[storage/data|data store]], which is where account information is written to and read from.

## Configuration

[Section titled “Configuration”](#configuration)

The internal directory is selected by leaving [[ref/object/authentication#directoryid|`directoryId`]] unset on the [[ref/object/authentication|Authentication]] singleton (found in the WebUI under  Settings ›  Authentication › General). In that configuration Stalwart reads account records from the [[ref/object/data-store|DataStore]] singleton (found in the WebUI under  Settings ›  Storage › Data Store), and no external Directory object is required.

## Account management

[Section titled “Account management”](#account-management)

Account management is performed through the [[management/webui|WebUI]], the JMAP API, or the [[management/cli|CLI]], all of which operate on the same underlying objects ([[ref/object/account|Account]], [[ref/object/account-password|AccountPassword]], [[ref/object/app-password|AppPassword]], [[ref/object/api-key|ApiKey]], and related principals).