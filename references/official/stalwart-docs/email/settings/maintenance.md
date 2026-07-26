---
title: "Maintenance"
source: "https://stalw.art/docs/email/settings/maintenance"
source_format: official-html-to-markdown
language: "en"
doc_slug: "email/settings/maintenance"
category: "email"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Maintenance"
---
# Maintenance

      

Stalwart includes an automated maintenance process that manages and optimises the use of server space. The process runs on a schedule to maintain the performance and reliability of the mail server, and performs several functions across user accounts.

It permanently removes emails that users have marked for deletion, reclaiming space occupied by unwanted emails. It expunges old messages from the “Deleted Items” and “Junk Mail” folders on a retention policy, preventing these folders from accumulating indefinitely. It also removes old entries from the changelog used by synchronisation features such as the IMAP CONDSTORE extension and JMAP, which keeps the synchronisation mechanisms efficient and bounds their storage cost.

Maintenance schedules and retention durations are configured on the [[ref/object/data-retention|DataRetention]] singleton (found in the WebUI under  Settings ›  Storage › Data Retention › Archiving,  Settings ›  Storage › Data Retention › Data Cleanup,  Settings ›  Storage › Data Retention › Auto-Expunge,  Settings ›  Storage › Data Retention › Telemetry).

## Schedule

[Section titled “Schedule”](#schedule)

The data-store clean-up schedule is controlled by [[ref/object/data-retention#datacleanupschedule|`dataCleanupSchedule`]], which takes a `Cron` value. The default runs daily at 02:00.

For example, to run the clean-up every day at midnight:

```
{  "dataCleanupSchedule": {"@type": "Daily", "hour": 0, "minute": 0}}
```

Related schedules on the same object include [[ref/object/data-retention#expungeschedule|`expungeSchedule`]] (auto-expunge run frequency, default daily at 00:00) and [[ref/object/data-retention#blobcleanupschedule|`blobCleanupSchedule`]] (blob-store clean-up, default daily at 04:00).

## Auto-Expunge

[Section titled “Auto-Expunge”](#auto-expunge)

Auto-expunge automates the management of email storage by systematically removing older messages from the “Deleted Items” and “Junk Mail” folders within all user accounts. This keeps mailboxes organised and bounds server storage usage.

The retention period is controlled by [[ref/object/data-retention#expungetrashafter|`expungeTrashAfter`]], which expects a duration. The default is `"30d"`: messages older than thirty days are expunged from those folders on the next run of [[ref/object/data-retention#expungeschedule|`expungeSchedule`]]. Setting [[ref/object/data-retention#expungetrashafter|`expungeTrashAfter`]] to `null` disables auto-expunge.

For example, to retain deleted and junk messages for thirty days before expunging:

```
{  "expungeTrashAfter": "30d"}
```

## Changes History

[Section titled “Changes History”](#changes-history)

Stalwart maintains a changelog, known as the changes history, which is used for synchronisation between the server and client email applications. The changelog is consumed by modern email clients over JMAP and by IMAP clients that support the `CONDSTORE`/`QRESYNC` extensions.

The changes history records modifications to mailboxes and messages, allowing clients to synchronise changes efficiently without fetching a full snapshot of the mailbox state each time. Keeping the changelog bounded conserves disk space and maintains server performance.

The number of changes retained per account is controlled by [[ref/object/data-retention#maxchangeshistory|`maxChangesHistory`]], which defaults to `10000`. Setting the value to `null` disables the cap.

For example:

```
{  "maxChangesHistory": 10000}
```