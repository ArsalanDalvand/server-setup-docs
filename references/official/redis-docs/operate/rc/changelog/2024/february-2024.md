---
title: "Redis Cloud changelog (February 2024)"
source: "https://redis.io/docs/latest/operate/rc/changelog/2024/february-2024/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/2024/february-2024"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (February 2024)"
---
# Redis Cloud changelog (February 2024)

New features, enhancements, and other changes added to Redis Cloud during February 2024.

            Redis Cloud
          

## Enhancements

### Dataset size alert set by default

The Dataset size has reached [alert](/docs/latest/operate/rc/databases/create-database/#alerts-section) is now set by default when you create your first database in a Flexible subscription.

### Separated capability selection

When you select [advanced capabilities](/docs/latest/operate/rc/databases/create-database/#modules) during database creation, selecting Search and query will no longer automatically select JSON. You can still combine the Search and query and JSON advanced capabilities.

### New backup location errors

The Redis Cloud console will now notify you through email and on the application if the backup location you specify does not exist or has the wrong permissions. See [Back up databases](/docs/latest/operate/rc/databases/back-up-data/) to learn how to set the correct permissions for your backup locations.

### CIDR overlap detection

When you [create a new Flexible subscription](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/), the Redis Cloud console will now detect if the Classless Inter-Domain Routing (CIDR) subnet range you specify is in use by another Flexible subscription on the same account. This will allow you to address and resolve CIDR conflicts in your Redis deployment.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page