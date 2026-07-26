---
title: "Overview"
source: "https://stalw.art/docs/server/dns"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/dns"
category: "server"
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

      

Stalwart can integrate with an external DNS provider and manage the records a mail server needs (MX, SPF, DKIM, DMARC, TLSA, autoconfig and autodiscover records, and the rest of the set described below) on the operator’s behalf. When this integration is active, the server publishes and updates the zone directly against the provider’s API, so records stay in sync with the server’s configuration without manual changes in the provider’s control panel.

## How records are provisioned

[Section titled “How records are provisioned”](#how-records-are-provisioned)

Integration is defined by the [[ref/object/dns-server|DnsServer]] object (found in the WebUI under  Settings ›  Network › DNS › DNS Providers), which stores the provider type and the credentials used to authenticate against its API. A single deployment can hold any number of DnsServer entries, for example one per provider or one per tenant.

A [[ref/object/domain|Domain]] (found in the WebUI under  Management ›  Domains › Domains) is wired to a provider through its [[ref/object/domain#dnsmanagement|`dnsManagement`]] field. Selecting the `Automatic` variant and setting [[ref/object/domain#dnsmanagement|`dnsServerId`]] to the identifier of a DnsServer activates the integration for that domain. When the Domain is first created with an `Automatic` [[ref/object/domain#dnsmanagement|`dnsManagement`]] configuration, Stalwart publishes the initial set of records into the target zone. The record types that are published and kept in sync are selected by the `publishRecords` list on the `Automatic` variant, which defaults to DKIM, SPF, MX, DMARC, SRV, MTA-STS, TLS-RPT, CAA, autoconfig, legacy autoconfig, and Microsoft autodiscover. TLSA records can also be added to that list. See [[domains/dns-records|DNS record setup]] for the full wiring.

## Refreshing records

[Section titled “Refreshing records”](#refreshing-records)

Records are kept current by the task manager. The [[ref/object/task|Task]] object exposes a `DnsManagement` variant that targets a specific [[ref/object/domain|Domain]] and optionally narrows the refresh to a subset of record types through its `updateRecords` field. Triggering a task of this variant causes Stalwart to reconcile the domain’s zone with the current configuration, publishing any missing records and replacing any stale ones. The same variant is used internally when DKIM keys are rotated or when the set of managed records changes. See [[management/tasks-actions/tasks|Scheduled tasks]] for how tasks are scheduled, triggered, and monitored.

For the list of supported providers and protocols, see [[server/dns/provider|Supported providers]].