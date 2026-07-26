---
title: "Tasks"
source: "https://stalw.art/docs/management/tasks-actions/tasks"
source_format: official-html-to-markdown
language: "en"
doc_slug: "management/tasks-actions/tasks"
category: "management"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Tasks"
---
# Tasks

      

A Task is a background job scheduled for execution. Tasks cover the recurring or deferred work the server performs outside the request path: search indexing, calendar alarm dispatch, outbound DMARC and TLS reports, account destruction, store and spam filter maintenance, ACME renewal, DKIM key rotation, and DNS record updates. Each scheduled job is represented as a [[ref/object/task|Task]] object (found in the WebUI under  Management ›  Tasks › Scheduled,  Management ›  Tasks › Failed) of one of the variants described below, and whose [[ref/object/task#status|`status`]] records whether the task is pending, awaiting retry, or has failed permanently.

Tasks are visible and manageable from the [[management/webui|WebUI]] (Management › Tasks › Scheduled and Management › Tasks › Failed), through the [[management/cli|CLI]] using `stalwart-cli get task`, `stalwart-cli query task`, and `stalwart-cli create task/<variant-name>`, and over the JMAP API through the `x:Task/get`, `x:Task/query`, and `x:Task/set` methods. Administrators can reschedule, retry, or destroy individual tasks through the same interfaces.

## Variants

[Section titled “Variants”](#variants)

Every variant of the [[ref/object/task|Task]] object carries a [[ref/object/task#status|`status`]] and a [[ref/object/task#due|`due`]] timestamp; variants specific to an account, domain, or other object carry the corresponding identifier. The full schema for each is on the reference page.

### Index document

[Section titled “Index document”](#index-document)

The `IndexDocument` variant indexes a single document for search. The caller supplies the owning [[ref/object/task#accountid|`accountId`]], the [[ref/object/task#documentid|`documentId`]], and the [[ref/object/task#documenttype|`documentType`]], which selects between email, calendar, contacts, and file.

### Unindex document

[Section titled “Unindex document”](#unindex-document)

The `UnindexDocument` variant removes a single document from the search index. The caller supplies the owning [[ref/object/task#accountid|`accountId`]], the [[ref/object/task#documentid|`documentId`]], and the [[ref/object/task#documenttype|`documentType`]] matching the previously indexed document.

### Index trace

[Section titled “Index trace”](#index-trace)

The `IndexTrace` variant indexes a telemetry trace so that it becomes queryable. The caller supplies the [[ref/object/task#traceid|`traceId`]] of the trace to index.

### Calendar alarm email

[Section titled “Calendar alarm email”](#calendar-alarm-email)

The `CalendarAlarmEmail` variant sends the email notification for a calendar alarm at the scheduled time. The server populates the alarm and event identifiers together with the event start and end timestamps and their timezone identifiers, and records the owning [[ref/object/task#accountid|`accountId`]] and [[ref/object/task#documentid|`documentId`]] of the calendar event.

### Calendar alarm notification

[Section titled “Calendar alarm notification”](#calendar-alarm-notification)

The `CalendarAlarmNotification` variant delivers the in-app notification for a calendar alarm at the scheduled time. Alongside the alarm and event identifiers, the server records an optional [[ref/object/task#recurrenceid|`recurrenceId`]] when the alarm targets a specific occurrence of a recurring event.

### Calendar iTIP message

[Section titled “Calendar iTIP message”](#calendar-itip-message)

The `CalendarItipMessage` variant delivers one or more iTIP messages for a calendar event to their recipients. The server populates [[ref/object/task#messages|`messages`]] with the per-recipient iTIP payloads together with the owning [[ref/object/task#accountid|`accountId`]] and [[ref/object/task#documentid|`documentId`]].

### Merge threads

[Section titled “Merge threads”](#merge-threads)

The `MergeThreads` variant merges email messages that share a thread identifier into a single thread. The server records the owning [[ref/object/task#accountid|`accountId`]], the [[ref/object/task#threadname|`threadName`]], and the list of [[ref/object/task#messageids|`messageIds`]] to fold into the thread.

### DMARC report

[Section titled “DMARC report”](#dmarc-report)

The `DmarcReport` variant submits a DMARC aggregate report to the remote reporting address for a reporting window. The task carries the [[ref/object/task#reportid|`reportId`]] of the internal report object holding the aggregated data.

### TLS report

[Section titled “TLS report”](#tls-report)

The `TlsReport` variant submits a TLS aggregate report to the remote reporting address for a reporting window. The task carries the [[ref/object/task#reportid|`reportId`]] of the internal TLS report object holding the aggregated data.

### Restore archived item

[Section titled “Restore archived item”](#restore-archived-item)

The `RestoreArchivedItem` variant restores an archived item into the owning account before its archive retention expires. The task carries the [[ref/object/task#blobid|`blobId`]] of the archived payload, the [[ref/object/task#archiveditemtype|`archivedItemType`]] (email, file, calendar event, contact card, or Sieve script), the original [[ref/object/task#createdat|`createdAt`]] timestamp, the [[ref/object/task#archiveduntil|`archivedUntil`]] deadline after which the item is deleted permanently, and the destination [[ref/object/task#accountid|`accountId`]].

### Destroy account

[Section titled “Destroy account”](#destroy-account)

The `DestroyAccount` variant permanently deletes an account and all data associated with it. The caller supplies the [[ref/object/task#accountname|`accountName`]] and [[ref/object/task#accountdomainid|`accountDomainId`]]; the server records the resolved [[ref/object/task#accountid|`accountId`]] and [[ref/object/task#accounttype|`accountType`]], either user or group.

### Account maintenance

[Section titled “Account maintenance”](#account-maintenance)

The `AccountMaintenance` variant runs a per-account maintenance operation selected by [[ref/object/task#maintenancetype|`maintenanceType`]]: purging expired data, reindexing for search, recalculating IMAP UIDs, or recalculating storage quota usage. The task carries the owning [[ref/object/task#accountid|`accountId`]].

### Tenant maintenance

[Section titled “Tenant maintenance”](#tenant-maintenance)

The `TenantMaintenance` variant runs a per-tenant maintenance operation selected by [[ref/object/task#maintenancetype|`maintenanceType`]]; currently, the only operation is recalculating tenant-wide storage quota usage. The task carries the owning [[ref/object/task#tenantid|`tenantId`]].

### Store maintenance

[Section titled “Store maintenance”](#store-maintenance)

The `StoreMaintenance` variant runs a store-wide maintenance operation selected by [[ref/object/task#maintenancetype|`maintenanceType`]]: reindexing all accounts or telemetry data, purging the data or blob stores, resetting user, tenant, or blob quotas, resetting rate limiters, or removing stale ACME and OAuth tokens, MTA queue locks, task manager locks, DAV locks, Sieve vacation and duplicate ID lists, or greylist entries. For sharded stores, an optional [[ref/object/task#shardindex|`shardIndex`]] selects the target shard.

### Spam filter maintenance

[Section titled “Spam filter maintenance”](#spam-filter-maintenance)

The `SpamFilterMaintenance` variant operates on the spam classifier according to [[ref/object/task#maintenancetype|`maintenanceType`]]: training on the latest samples, retraining on all available samples, aborting an ongoing training run, resetting the classifier model to its default state, or downloading and applying rule updates from the configured source.

### ACME renewal

[Section titled “ACME renewal”](#acme-renewal)

The `AcmeRenewal` variant renews the ACME-issued TLS certificate for a domain. The task carries the target [[ref/object/task#domainid|`domainId`]].

### DKIM rotation

[Section titled “DKIM rotation”](#dkim-rotation)

The `DkimManagement` variant performs DKIM key rotation for a domain. The task carries the target [[ref/object/task#domainid|`domainId`]].

### DNS record update

[Section titled “DNS record update”](#dns-record-update)

The `DnsManagement` variant updates one or more DNS record types for a domain. The [[ref/object/task#updaterecords|`updateRecords`]] field selects from DKIM public keys, TLSA, SPF, MX, DMARC, SRV, MTA-STS, TLS-RPT, CAA, Autoconfig (including the legacy variant), and Microsoft Autodiscover records, and [[ref/object/task#onsuccessrenewcertificate|`onSuccessRenewCertificate`]] optionally triggers an ACME renewal after a successful update. The task carries the target [[ref/object/task#domainid|`domainId`]].

## Retry status

[Section titled “Retry status”](#retry-status)

Every task carries a [[ref/object/task#status|`status`]] field drawn from the `TaskStatus` nested type. A task awaiting its first execution is `Pending` and carries a [[ref/object/task#taskstatuspending|`due`]] timestamp. A task that has failed but is still within its retry budget becomes `Retry` and records the [[ref/object/task#taskstatusretry|`attemptNumber`]] and the [[ref/object/task#taskstatusretry|`failureReason`]] from the last attempt. A task that has exhausted its retry budget becomes `Failed` and records the [[ref/object/task#taskstatusfailed|`failedAt`]] timestamp, the [[ref/object/task#taskstatusfailed|`failedAttemptNumber`]], and the terminal [[ref/object/task#taskstatusfailed|`failureReason`]].

## Task Manager

[Section titled “Task Manager”](#task-manager)

Global settings controlling how tasks are retried are held on the [[ref/object/task-manager|TaskManager]] singleton (found in the WebUI under  Settings ›  Task Manager). The relevant fields are:

[[ref/object/task-manager#maxattempts|`maxAttempts`]]: maximum number of attempts for a task before it is marked as permanently failed. Default `3`, minimum `1`.
[[ref/object/task-manager#strategy|`strategy`]]: retry strategy applied to failed tasks. Either the `ExponentialBackoff` variant, configured with a [[ref/object/task-manager#taskretrystrategybackoff|`factor`]], an [[ref/object/task-manager#taskretrystrategybackoff|`initialDelay`]], a [[ref/object/task-manager#taskretrystrategybackoff|`maxDelay`]], and an optional [[ref/object/task-manager#taskretrystrategybackoff|`jitter`]] toggle, or the `FixedDelay` variant, configured with a single [[ref/object/task-manager#taskretrystrategyfixed|`delay`]].
[[ref/object/task-manager#totaldeadline|`totalDeadline`]]: total time budget across all attempts before a task is marked as failed regardless of remaining attempts. Default `"6h"`.

Example using exponential backoff with jitter:

```
{  "maxAttempts": 5,  "strategy": {    "@type": "ExponentialBackoff",    "factor": 2.0,    "initialDelay": "1m",    "maxDelay": "30m",    "jitter": true  },  "totalDeadline": "6h"}
```

TaskManager settings are read through `x:TaskManager/get` with id `singleton` and updated through `x:TaskManager/set` (only the `update` argument is accepted). The CLI equivalents are `stalwart-cli get task-manager` and `stalwart-cli update task-manager`. Access is gated by the `sysTaskManagerGet` and `sysTaskManagerUpdate` permissions.