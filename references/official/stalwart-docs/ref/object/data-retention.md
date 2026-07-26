---
title: "DataRetention"
source: "https://stalw.art/docs/ref/object/data-retention"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/data-retention"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DataRetention"
---
# DataRetention

      

Configures data retention policies, expunge schedules, and archival settings.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Storage › Data Retention › Archiving Settings ›  Storage › Data Retention › Data Cleanup Settings ›  Storage › Data Retention › Auto-Expunge Settings ›  Storage › Data Retention › Telemetry

## Fields

[Section titled “Fields”](#fields)

##### expungeTrashAfter

[Section titled “expungeTrashAfter”](#expungetrashafter)

Type: `Duration?` · default: `"30d"`

How long to keep messages in the Trash and Junk Mail folders before auto-expunging

##### expungeSubmissionsAfter

[Section titled “expungeSubmissionsAfter”](#expungesubmissionsafter)

Type: `Duration?` · default: `"3d"`

How long to keep sent e-mail submissions before auto-expunging

##### expungeShareNotifyAfter

[Section titled “expungeShareNotifyAfter”](#expungesharenotifyafter)

Type: `Duration?` · default: `"30d"`

Specifies the duration for which the JMAP share notification history is retained before it is automatically purged.

##### expungeSchedulingInboxAfter

[Section titled “expungeSchedulingInboxAfter”](#expungeschedulinginboxafter)

Type: `Duration?` · default: `"30d"`

Sets the duration after which the iTIP inbox will automatically expunge old messages.

##### expungeSchedule

[Section titled “expungeSchedule”](#expungeschedule)

Type: [`Cron`](#cron) · default: `{"@type":"Daily","hour":0,"minute":0}`

Specifies when to run the auto-expunge process

##### dataCleanupSchedule

[Section titled “dataCleanupSchedule”](#datacleanupschedule)

Type: [`Cron`](#cron) · default: `{"@type":"Daily","hour":2,"minute":0}`

How often to purge the data store. Expects a cron expression.

##### blobCleanupSchedule

[Section titled “blobCleanupSchedule”](#blobcleanupschedule)

Type: [`Cron`](#cron) · default: `{"@type":"Daily","hour":4,"minute":0}`

How often to purge the data store. Expects a cron expression.

##### maxChangesHistory

[Section titled “maxChangesHistory”](#maxchangeshistory)

Type: `UnsignedInt?` · default: `10000`

How many changes to keep in the history for each account. This is used to determine the changes that have occurred since the last time the client requested changes.

##### archiveDeletedItemsFor

[Section titled “archiveDeletedItemsFor”](#archivedeleteditemsfor)

Type: `Duration?` · [[server/enterprise|enterprise]]

How long to keep deleted items in the archive before they are permanently deleted. If null, deleted items will not be archived and will be permanently deleted immediately.

##### archiveDeletedAccountsFor

[Section titled “archiveDeletedAccountsFor”](#archivedeletedaccountsfor)

Type: `Duration?` · [[server/enterprise|enterprise]]

How long to keep deleted accounts in the archive before they are permanently deleted. If null, deleted accounts will be permanently deleted immediately.

##### holdMtaReportsFor

[Section titled “holdMtaReportsFor”](#holdmtareportsfor)

Type: `Duration?` · default: `"30d"`

The duration for which MTA reports should be stored before being deleted, or None to disable storage

##### holdTracesFor

[Section titled “holdTracesFor”](#holdtracesfor)

Type: `Duration?` · [[server/enterprise|enterprise]] · default: `"30d"`

How long to keep message delivery history before it is permanently deleted.

##### holdMetricsFor

[Section titled “holdMetricsFor”](#holdmetricsfor)

Type: `Duration?` · [[server/enterprise|enterprise]] · default: `"90d"`

How long to keep metrics history before it is permanently deleted.

##### metricsCollectionInterval

[Section titled “metricsCollectionInterval”](#metricscollectioninterval)

Type: [`Cron`](#cron) · [[server/enterprise|enterprise]] · default: `{"@type":"Hourly","minute":0}`

Specifies how often to collect metrics history.

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The DataRetention singleton is available via the `urn:stalwart:jmap` capability.

### x:DataRetention/get

[Section titled “x:DataRetention/get”](#xdataretentionget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysDataRetentionGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DataRetention/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:DataRetention/set

[Section titled “x:DataRetention/set”](#xdataretentionset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysDataRetentionUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:DataRetention/set",          {            "update": {              "singleton": {                "expungeTrashAfter": "30d"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get DataRetention
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update DataRetention --field expungeTrashAfter=30d
```

## Nested types

[Section titled “Nested types”](#nested-types)

### Cron

[Section titled “Cron”](#cron)

Defines a recurring schedule.

`Daily`: Every day. Carries the fields of [`CronDaily`](#crondaily).
`Weekly`: Every week. Carries the fields of [`CronWeekly`](#cronweekly).
`Hourly`: Every hour. Carries the fields of [`CronHourly`](#cronhourly).

#### CronDaily

[Section titled “CronDaily”](#crondaily)

A daily recurring schedule at a specific time.

##### hour

[Section titled “hour”](#hour)

Type: `UnsignedInt` · default: `0` · max: 23

Hour

##### minute

[Section titled “minute”](#minute)

Type: `UnsignedInt` · default: `0` · max: 59

Minute

#### CronWeekly

[Section titled “CronWeekly”](#cronweekly)

A weekly recurring schedule on a specific day and time.

##### day

[Section titled “day”](#day)

Type: `UnsignedInt` · default: `0` · max: 6

Day

##### hour

[Section titled “hour”](#hour-1)

Type: `UnsignedInt` · default: `0` · max: 23

Hour

##### minute

[Section titled “minute”](#minute-1)

Type: `UnsignedInt` · default: `0` · max: 59

Minute

#### CronHourly

[Section titled “CronHourly”](#cronhourly)

An hourly recurring schedule at a specific minute.

##### minute

[Section titled “minute”](#minute-2)

Type: `UnsignedInt` · default: `0` · max: 59

Minute