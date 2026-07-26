---
title: "DNS records"
source: "https://stalw.art/docs/domains/dns-records"
source_format: official-html-to-markdown
language: "en"
doc_slug: "domains/dns-records"
category: "domains"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DNS records"
---
# DNS records

      

Stalwart can publish and maintain a domain’s DNS records directly against a managed zone, removing the need to copy records into a provider web console by hand. The feature is driven by the Domain object and relies on a separately configured [[ref/object/dns-server|DnsServer]] (found in the WebUI under  Settings ›  Network › DNS › DNS Providers) that carries the credentials and transport details for the zone. See [[server/dns|DNS providers]] for the DnsServer setup itself; this page covers the lifecycle once a DnsServer is in place.

## Enabling automatic DNS management

[Section titled “Enabling automatic DNS management”](#enabling-automatic-dns-management)

Automatic management is selected per domain through the [[ref/object/domain#dnsmanagement|`dnsManagement`]] field on the [[ref/object/domain|Domain]] object. The `Manual` variant leaves the operator responsible for publishing records from the [[ref/object/domain#dnszonefile|`dnsZoneFile`]] text into the external provider. The `Automatic` variant delegates that work to the server and carries three settings:

[[ref/object/domain#dnsserverid|`dnsServerId`]] references the DnsServer the domain should publish through.
[[ref/object/domain#origin|`origin`]] names the zone apex when it differs from the domain itself. For a mail domain of `sub.example.com` hosted inside the `example.com` zone, the origin is set to `example.com`. Leaving the field unset falls back to the domain name.
[[ref/object/domain#publishrecords|`publishRecords`]] lists the record types the server is allowed to write. The default covers `dkim`, `spf`, `mx`, `dmarc`, `srv`, `mtaSts`, `tlsRpt`, `caa`, `autoConfig`, `autoConfigLegacy`, and `autoDiscover`. Trimming the list restricts the server to a subset, which is useful when some record types are managed elsewhere.

## Initial publication

[Section titled “Initial publication”](#initial-publication)

Records are written to the zone the first time a Domain is saved with automatic DNS management enabled. Creating a Domain with the `Automatic` variant of [[ref/object/domain#dnsmanagement|`dnsManagement`]] triggers a publication pass that adds each record type in [[ref/object/domain#publishrecords|`publishRecords`]] to the configured zone. Switching an existing Domain from manual to automatic management has the same effect: the next save reconciles the expected record set with what is currently present in the zone and adds anything missing.

Propagation is driven by the DnsServer’s polling and timeout settings. The server issues the update, waits for the configured [[ref/object/dns-server#propagationdelay|`propagationDelay`]], then polls every [[ref/object/dns-server#pollinginterval|`pollingInterval`]] until the record is observed or [[ref/object/dns-server#propagationtimeout|`propagationTimeout`]] is reached. Records are written with a TTL of [[ref/object/dns-server#ttl|`ttl`]].

Flipping a Domain to automatic DNS management does not block the save: the server schedules a `DnsManagement` [[ref/object/task|Task]] (found in the WebUI under  Management ›  Tasks › Scheduled,  Management ›  Tasks › Failed) that performs the initial publication asynchronously. The task’s progress and outcome are visible on the Task record; if publication fails, the task is marked failed and its [[ref/object/task#failurereason|`failureReason`]] carries the details for diagnosis and retry.

## Refreshing records

[Section titled “Refreshing records”](#refreshing-records)

After initial publication, the record set is kept in sync by running the DNS management task. The task is a variant of the [[ref/object/task|Task]] object (the `DnsManagement` variant) and can be triggered manually when a refresh is required, for example after editing the domain configuration, changing the report address, or rotating DKIM keys. In the WebUI, tasks are created from Management > Tasks via the New Task button; from the CLI, the Task object is created with `stalwart-cli` in the same way as any other object. Triggering the task causes the server to rebuild the expected record set for the domain, compare it to the live zone, and issue the required add, update, and delete operations against the DnsServer.

The task carries two fields of operational interest. [[ref/object/task#updaterecords|`updateRecords`]] restricts the refresh to a subset of record types, which is useful when only a specific record (for example, DKIM) needs to be rewritten. [[ref/object/task#onsuccessrenewcertificate|`onSuccessRenewCertificate`]] chains an ACME renewal after a successful DNS update, which is relevant when the zone change is a prerequisite for a DNS-01 challenge.

## Record coverage

[Section titled “Record coverage”](#record-coverage)

The record types the server can manage are described by the `DnsRecordType` enumeration on the Domain object: `dkim`, `tlsa`, `spf`, `mx`, `dmarc`, `srv`, `mtaSts`, `tlsRpt`, `caa`, `autoConfig`, `autoConfigLegacy`, and `autoDiscover`. The contents of each record are derived from the Domain and from related objects (for example, DKIM public keys come from the [[ref/object/dkim-signature|DkimSignature]] records for the domain). The current expected zone content is always available on the Domain through the read-only [[ref/object/domain#dnszonefile|`dnsZoneFile`]] field.