---
title: "Redis Cloud changelog (December 2023)"
source: "https://redis.io/docs/latest/operate/rc/changelog/2023/december-2023/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/2023/december-2023"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (December 2023)"
---
# Redis Cloud changelog (December 2023)

New features, enhancements, and other changes added to Redis Cloud during December 2023.

            Redis Cloud
          

## New features

### Active-Active JSON support

[Active-Active databases](/docs/latest/operate/rc/databases/active-active/) on Redis Cloud now support the [JSON](/docs/latest/operate/oss_and_stack/stack-with-enterprise/json/) data type.

See [Create an Active-Active subscription](/docs/latest/operate/rc/databases/active-active/create-active-active-database/) to learn how to create an Active-Active subscription.

### Mutual TLS enhancements

Databases that support [[operate/rc/security/database-security/tls-ssl|Transport layer security (TLS)]] now support multiple client certificates for use with mutual TLS. This makes it easier to rotate client certificates outside of a maintenance window. In addition, you can now provide a client Certificate Authority chain to trust any leaf certificate it signed for more flexibility.

See [[operate/rc/security/database-security/tls-ssl|Transport layer security (TLS)]] to learn how to enable TLS.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page