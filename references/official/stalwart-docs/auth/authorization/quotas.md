---
title: "Quotas"
source: "https://stalw.art/docs/auth/authorization/quotas"
source_format: official-html-to-markdown
language: "en"
doc_slug: "auth/authorization/quotas"
category: "auth"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Quotas"
---
# Quotas

      

Quotas regulate resource consumption on the server, ensuring fair allocation of storage and object capacity across accounts and tenants. They define limits on the amount of data an account may hold, as well as the number of objects of a given kind it may create. Quotas can be enforced per account or, in multi-tenant deployments, per tenant.

Stalwart supports two broad categories of quota. Disk-usage quotas control the total storage consumed by an account’s data, while object quotas cap the number of distinct items (messages, mailboxes, calendar events, address book entries, and similar) that an account can hold.

Per-account limits are carried on the [[ref/object/account|Account]] object (found in the WebUI under  Management ›  Directory › Groups,  Management ›  Directory › Accounts) via its [[ref/object/account#quotas|`quotas`]] field, which is a map from a `StorageQuota` key to a numeric limit. Per-tenant limits are carried on the [[ref/object/tenant|Tenant]] object (found in the WebUI under  Management ›  Directory › Tenants) via its [[ref/object/tenant#quotas|`quotas`]] field, which maps a `TenantStorageQuota` key to a numeric limit.

## Disk Usage Quotas

[Section titled “Disk Usage Quotas”](#disk-usage-quotas)

The total amount of disk space an account may consume is expressed through the `maxDiskQuota` entry in the account’s [[ref/object/account#quotas|`quotas`]] map, in bytes. Tenants expose an equivalent entry on their [[ref/object/tenant#quotas|`quotas`]] map, capping total storage across the tenant’s members.

Quota values are held exclusively on the Account and Tenant objects; the server does not read disk-quota attributes from external directories. Even when authentication is delegated to LDAP or SQL, quotas must be configured through the Account object (or the Tenant object for tenant-wide caps) from the WebUI or the JMAP API.

## Object Quotas

[Section titled “Object Quotas”](#object-quotas)

Object quotas cap the number of items of each kind that can be stored under an account or tenant. The available keys are defined by the `StorageQuota` enum on the Account object, and by the `TenantStorageQuota` enum on the Tenant object. Relevant `StorageQuota` values include [[ref/object/account#storagequota|`maxEmails`]], [[ref/object/account#storagequota|`maxMailboxes`]], [[ref/object/account#storagequota|`maxCalendars`]], [[ref/object/account#storagequota|`maxCalendarEvents`]], [[ref/object/account#storagequota|`maxAddressBooks`]], [[ref/object/account#storagequota|`maxContactCards`]], [[ref/object/account#storagequota|`maxFiles`]], [[ref/object/account#storagequota|`maxEmailIdentities`]], [[ref/object/account#storagequota|`maxEmailSubmissions`]], [[ref/object/account#storagequota|`maxSieveScripts`]], [[ref/object/account#storagequota|`maxPushSubscriptions`]], [[ref/object/account#storagequota|`maxAppPasswords`]], and [[ref/object/account#storagequota|`maxApiKeys`]].

A value set under an account’s [[ref/object/account#quotas|`quotas`]] map overrides the server-wide default for that key. The defaults themselves live on the service-specific singletons rather than on the Account object: mail-related caps such as [[ref/object/email#maxmessages|`maxMessages`]], [[ref/object/email#maxmailboxes|`maxMailboxes`]], [[ref/object/email#maxsubmissions|`maxSubmissions`]], and [[ref/object/email#maxidentities|`maxIdentities`]] are defined on the [[ref/object/email|Email]] singleton, calendar caps such as [[ref/object/calendar#maxcalendars|`maxCalendars`]] and [[ref/object/calendar#maxevents|`maxEvents`]] on the [[ref/object/calendar|Calendar]] singleton, address-book caps such as [[ref/object/address-book#maxaddressbooks|`maxAddressBooks`]] and [[ref/object/address-book#maxcontacts|`maxContacts`]] on the [[ref/object/address-book|AddressBook]] singleton, and file-storage caps such as [[ref/object/file-storage#maxfiles|`maxFiles`]] on the [[ref/object/file-storage|FileStorage]] singleton. Consult those objects for the server-wide default applied when an account does not set its own value.

To restrict the number of calendars and address books that a single account may create, set the relevant entries on the account’s [[ref/object/account#quotas|`quotas`]] field:

```
{  "quotas": {    "maxCalendars": 300,    "maxAddressBooks": 500  }}
```

Default maximum numbers of per-account app passwords and API keys are also controlled globally through [[ref/object/authentication#maxapppasswords|`maxAppPasswords`]] and [[ref/object/authentication#maxapikeys|`maxApiKeys`]] on the [[ref/object/authentication|Authentication]] singleton (found in the WebUI under  Settings ›  Authentication › General).