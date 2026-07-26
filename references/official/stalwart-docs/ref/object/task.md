---
title: "Task"
source: "https://stalw.art/docs/ref/object/task"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/task"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Task"
---
# Task

      

Represents a background task scheduled for execution.

This object can be configured from the [[management/webui|WebUI]] under  Management ›  Tasks › Scheduled Management ›  Tasks › Failed

## Fields

[Section titled “Fields”](#fields)

Task is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "IndexDocument"

[Section titled “@type: "IndexDocument"”](#type-indexdocument)

Index document

##### documentType

[Section titled “documentType”](#documenttype)

Type: [`IndexDocumentType`](#indexdocumenttype) · read-only

Type of document associated with the task

##### accountId

[Section titled “accountId”](#accountid)

Type: `Id<`[[ref/object/account|`Account`]]`>` · read-only

Identifier of the account associated with this task

##### documentId

[Section titled “documentId”](#documentid)

Type: `Id` · read-only

Identifier of the document associated with this task

##### status

[Section titled “status”](#status)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "UnindexDocument"

[Section titled “@type: "UnindexDocument"”](#type-unindexdocument)

Unindex document

##### documentType

[Section titled “documentType”](#documenttype-1)

Type: [`IndexDocumentType`](#indexdocumenttype) · read-only

Type of document associated with the task

##### accountId

[Section titled “accountId”](#accountid-1)

Type: `Id<`[[ref/object/account|`Account`]]`>` · read-only

Identifier of the account associated with this task

##### documentId

[Section titled “documentId”](#documentid-1)

Type: `Id` · read-only

Identifier of the document associated with this task

##### status

[Section titled “status”](#status-1)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-1)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "IndexTrace"

[Section titled “@type: "IndexTrace"”](#type-indextrace)

Index telemetry trace

##### traceId

[Section titled “traceId”](#traceid)

Type: `Id<`[[ref/object/trace|`Trace`]]`>` · read-only

Identifier of the trace associated with this task

##### status

[Section titled “status”](#status-2)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-2)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "CalendarAlarmEmail"

[Section titled “@type: "CalendarAlarmEmail"”](#type-calendaralarmemail)

Calendar alarm e-mail

##### alarmId

[Section titled “alarmId”](#alarmid)

Type: `UnsignedInt` · server-set · default: `0`

Identifier of the calendar alarm associated with this task

##### eventId

[Section titled “eventId”](#eventid)

Type: `UnsignedInt` · server-set · default: `0`

Identifier of the calendar event associated with this task

##### eventStart

[Section titled “eventStart”](#eventstart)

Type: `UTCDateTime` · server-set

Start date and time of the calendar event

##### eventEnd

[Section titled “eventEnd”](#eventend)

Type: `UTCDateTime` · server-set

End date and time of the calendar event

##### eventStartTz

[Section titled “eventStartTz”](#eventstarttz)

Type: `UnsignedInt` · server-set · default: `0`

Timezone identifier for the start date and time

##### eventEndTz

[Section titled “eventEndTz”](#eventendtz)

Type: `UnsignedInt` · server-set · default: `0`

Timezone identifier for the end date and time

##### accountId

[Section titled “accountId”](#accountid-2)

Type: `Id<`[[ref/object/account|`Account`]]`>` · read-only

Identifier of the account associated with this task

##### documentId

[Section titled “documentId”](#documentid-2)

Type: `Id` · read-only

Identifier of the document associated with this task

##### status

[Section titled “status”](#status-3)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-3)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "CalendarAlarmNotification"

[Section titled “@type: "CalendarAlarmNotification"”](#type-calendaralarmnotification)

Calendar alarm notification

##### alarmId

[Section titled “alarmId”](#alarmid-1)

Type: `UnsignedInt` · server-set · default: `0`

Identifier of the calendar alarm associated with this task

##### eventId

[Section titled “eventId”](#eventid-1)

Type: `UnsignedInt` · server-set · default: `0`

Identifier of the calendar event associated with this task

##### recurrenceId

[Section titled “recurrenceId”](#recurrenceid)

Type: `Integer?` · server-set

Recurrence identifier for the alarm, if applicable

##### accountId

[Section titled “accountId”](#accountid-3)

Type: `Id<`[[ref/object/account|`Account`]]`>` · read-only

Identifier of the account associated with this task

##### documentId

[Section titled “documentId”](#documentid-3)

Type: `Id` · read-only

Identifier of the document associated with this task

##### status

[Section titled “status”](#status-4)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-4)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "CalendarItipMessage"

[Section titled “@type: "CalendarItipMessage"”](#type-calendaritipmessage)

Calendar iTIP message

##### messages

[Section titled “messages”](#messages)

Type: [`TaskCalendarItipContents`](#taskcalendaritipcontents)`[]` · server-set

List of iTIP messages associated with this task

##### accountId

[Section titled “accountId”](#accountid-4)

Type: `Id<`[[ref/object/account|`Account`]]`>` · read-only

Identifier of the account associated with this task

##### documentId

[Section titled “documentId”](#documentid-4)

Type: `Id` · read-only

Identifier of the document associated with this task

##### status

[Section titled “status”](#status-5)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-5)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "MergeThreads"

[Section titled “@type: "MergeThreads"”](#type-mergethreads)

Merge email threads

##### accountId

[Section titled “accountId”](#accountid-5)

Type: `Id<`[[ref/object/account|`Account`]]`>` · server-set

Identifier of the account associated with this task

##### threadName

[Section titled “threadName”](#threadname)

Type: `String` · server-set

Name of the thread to be merged

##### messageIds

[Section titled “messageIds”](#messageids)

Type: `String[]` · server-set

Message-IDs of the email messages to be merged into the thread

##### status

[Section titled “status”](#status-6)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-6)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "DmarcReport"

[Section titled “@type: "DmarcReport"”](#type-dmarcreport)

Send DMARC report to remote server

##### reportId

[Section titled “reportId”](#reportid)

Type: `Id<`[[ref/object/dmarc-internal-report|`DmarcInternalReport`]]`>` · server-set

Identifier for the DMARC aggregate report associated with this task

##### status

[Section titled “status”](#status-7)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-7)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "TlsReport"

[Section titled “@type: "TlsReport"”](#type-tlsreport)

Send TLS report to remote server

##### reportId

[Section titled “reportId”](#reportid-1)

Type: `Id<`[[ref/object/tls-internal-report|`TlsInternalReport`]]`>` · server-set

Identifier for the TLS aggregate report associated with this task

##### status

[Section titled “status”](#status-8)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-8)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "RestoreArchivedItem"

[Section titled “@type: "RestoreArchivedItem"”](#type-restorearchiveditem)

Restore archived item

##### blobId

[Section titled “blobId”](#blobid)

Type: `BlobId` · server-set

Identifier of the archived blob to be restored

##### archivedItemType

[Section titled “archivedItemType”](#archiveditemtype)

Type: [`ArchivedItemType`](#archiveditemtype) · server-set

Type of the archived item associated with the blob

##### createdAt

[Section titled “createdAt”](#createdat)

Type: `UTCDateTime` · server-set

Timestamp when the item was originally created

##### archivedUntil

[Section titled “archivedUntil”](#archiveduntil)

Type: `UTCDateTime` · server-set

Timestamp until which the archived item will be deleted permanently if not restored

##### accountId

[Section titled “accountId”](#accountid-6)

Type: `Id<`[[ref/object/account|`Account`]]`>` · server-set

Identifier of the account to which the archived item belongs

##### status

[Section titled “status”](#status-9)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-9)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "DestroyAccount"

[Section titled “@type: "DestroyAccount"”](#type-destroyaccount)

Destroy account and all associated data

##### accountId

[Section titled “accountId”](#accountid-7)

Type: `Id<`[[ref/object/account|`Account`]]`>` · server-set

Identifier of the account to be destroyed

##### accountName

[Section titled “accountName”](#accountname)

Type: `String` · required

Name of the account to be destroyed

##### accountDomainId

[Section titled “accountDomainId”](#accountdomainid)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · required

Domain identifier of the account to be destroyed, if applicable

##### accountType

[Section titled “accountType”](#accounttype)

Type: [`AccountType`](#accounttype) · server-set

Type of the deleted account (user or group)

##### status

[Section titled “status”](#status-10)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-10)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "AccountMaintenance"

[Section titled “@type: "AccountMaintenance"”](#type-accountmaintenance)

Perform account maintenance operations

##### accountId

[Section titled “accountId”](#accountid-8)

Type: `Id<`[[ref/object/account|`Account`]]`>` · read-only

Identifier of the account to be maintained

##### maintenanceType

[Section titled “maintenanceType”](#maintenancetype)

Type: [`TaskAccountMaintenanceType`](#taskaccountmaintenancetype) · read-only

Type of maintenance operation to perform on the account

##### status

[Section titled “status”](#status-11)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-11)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "TenantMaintenance"

[Section titled “@type: "TenantMaintenance"”](#type-tenantmaintenance)

Perform tenant maintenance operations

##### tenantId

[Section titled “tenantId”](#tenantid)

Type: `Id<`[[ref/object/tenant|`Tenant`]]`>` · read-only

Identifier of the tenant to be maintained

##### maintenanceType

[Section titled “maintenanceType”](#maintenancetype-1)

Type: [`TaskTenantMaintenanceType`](#tasktenantmaintenancetype) · read-only

Type of maintenance operation to perform on the tenant

##### status

[Section titled “status”](#status-12)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-12)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "StoreMaintenance"

[Section titled “@type: "StoreMaintenance"”](#type-storemaintenance)

Perform store maintenance operations

##### maintenanceType

[Section titled “maintenanceType”](#maintenancetype-2)

Type: [`TaskStoreMaintenanceType`](#taskstoremaintenancetype) · read-only

Type of maintenance operation to perform on the store

##### shardIndex

[Section titled “shardIndex”](#shardindex)

Type: `UnsignedInt?`

Index of the shard to perform maintenance on, if applicable for the maintenance type

##### status

[Section titled “status”](#status-13)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-13)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "SpamFilterMaintenance"

[Section titled “@type: "SpamFilterMaintenance"”](#type-spamfiltermaintenance)

Perform spam filter maintenance operations

##### maintenanceType

[Section titled “maintenanceType”](#maintenancetype-3)

Type: [`TaskSpamFilterMaintenanceType`](#taskspamfiltermaintenancetype) · read-only

Type of maintenance operation to perform on the spam filter

##### status

[Section titled “status”](#status-14)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-14)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "AcmeRenewal"

[Section titled “@type: "AcmeRenewal"”](#type-acmerenewal)

Perform ACME certificate renewal for a domain

##### domainId

[Section titled “domainId”](#domainid)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · read-only

Identifier of the domain associated with this task

##### status

[Section titled “status”](#status-15)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-15)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "DkimManagement"

[Section titled “@type: "DkimManagement"”](#type-dkimmanagement)

Perform DKIM key rotation for a domain

##### domainId

[Section titled “domainId”](#domainid-1)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · read-only

Identifier of the domain associated with this task

##### status

[Section titled “status”](#status-16)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-16)

Type: `UTCDateTime?` · server-set

Due date and time for the task

### @type: "DnsManagement"

[Section titled “@type: "DnsManagement"”](#type-dnsmanagement)

Perform DNS management for a domain

##### updateRecords

[Section titled “updateRecords”](#updaterecords)

Type: [`DnsRecordType`](#dnsrecordtype)`[]`

Which DNS records should be updated for the domain as part of this task

##### onSuccessRenewCertificate

[Section titled “onSuccessRenewCertificate”](#onsuccessrenewcertificate)

Type: `Boolean` · default: `false`

Whether to automatically renew the domain’s TLS certificate using ACME after successfully updating DNS records

##### domainId

[Section titled “domainId”](#domainid-2)

Type: `Id<`[[ref/object/domain|`Domain`]]`>` · read-only

Identifier of the domain associated with this task

##### status

[Section titled “status”](#status-17)

Type: [`TaskStatus`](#taskstatus) · required

Current status of the task

##### due

[Section titled “due”](#due-17)

Type: `UTCDateTime?` · server-set

Due date and time for the task

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The Task object is available via the `urn:stalwart:jmap` capability.

### x:Task/get

[Section titled “x:Task/get”](#xtaskget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysTaskGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Task/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Task/set

[Section titled “x:Task/set”](#xtaskset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysTaskCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Task/set",          {            "create": {              "new1": {                "@type": "IndexDocument",                "status": {                  "@type": "Pending",                  "due": "2026-01-01T00:00:00Z"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysTaskUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Task/set",          {            "update": {              "id1": {                "status": {                  "@type": "Pending",                  "due": "2026-01-01T00:00:00Z"                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysTaskDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Task/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Task/query

[Section titled “x:Task/query”](#xtaskquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysTaskQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Task/query",          {            "filter": {              "@type": "value"            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

The `x:Task/query` `filter` argument accepts the following conditions (combinable with `AnyOf` / `AllOf` / `Not` per RFC 8620):

ConditionKind`@type`enum: TaskType`status`enum: TaskStatusType`due`date

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Task id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create Task/IndexDocument \  --field 'status={"@type":"Pending","due":"2026-01-01T00:00:00Z"}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query Taskstalwart-cli query Task --where @type=value
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update Task id1 --field status='{"@type":"Pending","due":"2026-01-01T00:00:00Z"}'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete Task --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### TaskStatus

[Section titled “TaskStatus”](#taskstatus)

Execution status of a background task.

`Pending`: Pending task awaiting execution. Carries the fields of [`TaskStatusPending`](#taskstatuspending).
`Retry`: Task scheduled for retry. Carries the fields of [`TaskStatusRetry`](#taskstatusretry).
`Failed`: Failed task. Carries the fields of [`TaskStatusFailed`](#taskstatusfailed).

#### TaskStatusPending

[Section titled “TaskStatusPending”](#taskstatuspending)

Pending task status details.

##### createdAt

[Section titled “createdAt”](#createdat-1)

Type: `UTCDateTime` · server-set

Date and time when the task was created

##### due

[Section titled “due”](#due-18)

Type: `UTCDateTime` · required

Due date and time for the task

#### TaskStatusRetry

[Section titled “TaskStatusRetry”](#taskstatusretry)

Task retry status details.

##### createdAt

[Section titled “createdAt”](#createdat-2)

Type: `UTCDateTime` · server-set

Date and time when the task was created

##### due

[Section titled “due”](#due-19)

Type: `UTCDateTime` · required

Due date and time for the task

##### attemptNumber

[Section titled “attemptNumber”](#attemptnumber)

Type: `UnsignedInt` · default: `1`

Number of attempts made to complete the task

##### failureReason

[Section titled “failureReason”](#failurereason)

Type: `Text` · required

Reason for the last failure

#### TaskStatusFailed

[Section titled “TaskStatusFailed”](#taskstatusfailed)

Failed task status details.

##### createdAt

[Section titled “createdAt”](#createdat-3)

Type: `UTCDateTime` · server-set

Date and time when the task was created

##### failedAt

[Section titled “failedAt”](#failedat)

Type: `UTCDateTime` · required

Date and time when the task failed

##### failedAttemptNumber

[Section titled “failedAttemptNumber”](#failedattemptnumber)

Type: `UnsignedInt` · default: `0`

Number of attempts made before the task failed

##### failureReason

[Section titled “failureReason”](#failurereason-1)

Type: `Text` · required

Reason for task failure

### TaskCalendarItipContents

[Section titled “TaskCalendarItipContents”](#taskcalendaritipcontents)

Contents of an iTIP message to be delivered.

##### from

[Section titled “from”](#from)

Type: `EmailAddress` · server-set

Email address of the sender of the iTIP message

##### to

[Section titled “to”](#to)

Type: `EmailAddress[]` · server-set

Email addresses of the recipients of the iTIP message

##### isFromOrganizer

[Section titled “isFromOrganizer”](#isfromorganizer)

Type: `Boolean` · server-set · default: `false`

Indicates whether the sender is the organizer of the calendar event

##### iCalendarData

[Section titled “iCalendarData”](#icalendardata)

Type: `String` · server-set

iCalendar data associated with the iTIP message

##### summary

[Section titled “summary”](#summary)

Type: `String` · server-set

Summary of the calendar event associated with the iTIP message

## Enums

[Section titled “Enums”](#enums)

### IndexDocumentType

[Section titled “IndexDocumentType”](#indexdocumenttype)
ValueLabel`email`Email`calendar`Calendar`contacts`Contacts`file`File

### ArchivedItemType

[Section titled “ArchivedItemType”](#archiveditemtype-1)
ValueLabel`Email`Archived Email message`FileNode`Archived File`CalendarEvent`Archived Calendar Event`ContactCard`Archived Contact Card`SieveScript`Archived Sieve Script

### AccountType

[Section titled “AccountType”](#accounttype-1)
ValueLabel`User`User account`Group`Group account

### TaskAccountMaintenanceType

[Section titled “TaskAccountMaintenanceType”](#taskaccountmaintenancetype)
ValueLabel`purge`Purge expired data from the account`reindex`Reindex the account’s data for search`recalculateImapUid`Recalculate IMAP UIDs for the account’s email messages`recalculateQuota`Recalculate storage quota usage for the account

### TaskTenantMaintenanceType

[Section titled “TaskTenantMaintenanceType”](#tasktenantmaintenancetype)
ValueLabel`recalculateQuota`Recalculate storage quota usage for the tenant

### TaskStoreMaintenanceType

[Section titled “TaskStoreMaintenanceType”](#taskstoremaintenancetype)
ValueLabel`reindexAccounts`Reindex all accounts’ data for search`reindexTelemetry`Reindex all telemetry data for search`purgeAccounts`Purge expired data from all accounts`purgeData`Purge data store`purgeBlob`Purge blob store`resetRateLimiters`Reset all rate limiters`resetUserQuotas`Reset all user quotas`resetTenantQuotas`Reset all tenant quotas`resetBlobQuotas`Reset all blob quotas`removeAuthTokens`Delete all temporary ACME and OAuth tokens`removeLockQueueMessage`Delete all MTA queue message locks`removeLockTask`Delete all task manager locks`removeLockDav`Delete all DAV locks`removeSieveId`Delete all Sieve vacation and duplicate ID lists`removeGreylist`Delete all spam filter grey list entries

### TaskSpamFilterMaintenanceType

[Section titled “TaskSpamFilterMaintenanceType”](#taskspamfiltermaintenancetype)
ValueLabel`train`Train the spam classifier with the latest samples`retrain`Retrain the spam classifier with all available samples`abort`Abort the ongoing training process of the spam classifier`reset`Delete the spam classifier’s model and reset it to the default state`updateRules`Download and update the spam filter rules from the configured source

### DnsRecordType

[Section titled “DnsRecordType”](#dnsrecordtype)
ValueLabel`dkim`DKIM public keys`tlsa`TLSA records`spf`SPF records`mx`MX records`dmarc`DMARC policy`srv`SRV records`mtaSts`MTA-STS policy record`tlsRpt`TLS reporting record`caa`CAA records`autoConfig`Autoconfig records`autoConfigLegacy`Legacy Autoconfig records`autoDiscover`Microsoft Autodiscover records