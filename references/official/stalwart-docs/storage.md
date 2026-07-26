---
title: "Overview"
source: "https://stalw.art/docs/storage"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage"
category: "stalwart"
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

      

Stalwart splits storage into four separate stores, each covering a different aspect of server operation. Every store is an independent singleton with its own backend, so the storage technology can be matched to the requirements of each layer (a fast embedded database for metadata, an object-storage service for large blobs, a dedicated search index, and so on).

The [[storage/data|data store]] is the primary metadata store, configured through the [[ref/object/data-store|DataStore]] object (found in the WebUI under  Settings ›  Storage › Data Store). It holds every structured record the server needs: mailbox state, account settings, domains, calendars, contacts, and most configuration objects themselves.

The [[storage/blob|blob store]] is configured through the [[ref/object/blob-store|BlobStore]] object (found in the WebUI under  Settings ›  Storage › Blob Store) and holds the raw bytes of messages, attachments, Sieve scripts, and other large files. Small blobs below the per-backend threshold are kept inline in the data store; larger blobs spill into the blob store backend.

The [[storage/fts|search store]] is configured through the [[ref/object/search-store|SearchStore]] object (found in the WebUI under  Settings ›  Storage › Search Store) and indexes message bodies and attachments so that full-text search queries return quickly.

The [[storage/in-memory|in-memory store]] is configured through the [[ref/object/in-memory-store|InMemoryStore]] object (found in the WebUI under  Settings ›  Storage › In-Memory Store) and backs rate limiters, authentication tokens, session data, and other short-lived state.

Each object supports multiple backend variants (RocksDB, PostgreSQL, MySQL, SQLite, FoundationDB, S3, Redis, and others), and switching backend is a matter of selecting a different variant on the relevant object. Backend-specific options, defaults, and trade-offs are documented under [[storage/backends|Storage backends]].

## Clean-up and maintenance

[Section titled “Clean-up and maintenance”](#clean-up-and-maintenance)

Scheduled clean-up is not configured per store. It is managed centrally through the [[ref/object/data-retention|DataRetention]] object (found in the WebUI under  Settings ›  Storage › Data Retention › Archiving,  Settings ›  Storage › Data Retention › Data Cleanup,  Settings ›  Storage › Data Retention › Auto-Expunge,  Settings ›  Storage › Data Retention › Telemetry), which sets when the expunge, data-store clean-up, and blob clean-up tasks run. The relevant fields are [[ref/object/data-retention#expungeschedule|`expungeSchedule`]], [[ref/object/data-retention#datacleanupschedule|`dataCleanupSchedule`]] and [[ref/object/data-retention#blobcleanupschedule|`blobCleanupSchedule`]]. Default values are listed on the object reference page; each accepts a cron expression.

## Un-deleting emails

[Section titled “Un-deleting emails”](#un-deleting-emails)

Stalwart Enterprise edition provides a feature that allows recovery of emails deleted by users, even after these emails have been expunged from the Deleted Items or Junk Mail folders. This allows emails to be retrieved within a specified period, giving organisations a data recovery option.

### Configuration

[Section titled “Configuration”](#configuration)

The retention period is controlled by [[ref/object/data-retention#archivedeleteditemsfor|`archiveDeletedItemsFor`]] on the [[ref/object/data-retention|DataRetention]] object. The value is a duration, for example `"30d"` to retain deleted emails for thirty days before they are permanently removed. Leaving the field unset disables archiving, in which case deleted items are removed immediately. There is no separate enable flag; the presence or absence of a duration acts as the switch.

### Recovery

[Section titled “Recovery”](#recovery)

Archived items are stored under individual accounts, so recovery is available to both administrators (over every account they manage) and end users (over their own account). The [[management/webui|WebUI]] exposes archived items in the account view, from which they can be restored back into the mailbox. The same operation is available over the JMAP API as an `x:ArchivedItem/set` call on the [[ref/object/archived-item|ArchivedItem]] object and through the [[management/cli|CLI]].