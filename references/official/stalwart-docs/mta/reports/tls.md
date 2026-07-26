---
title: "TLS"
source: "https://stalw.art/docs/mta/reports/tls"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/reports/tls"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "TLS"
---
# TLS

      

TLS reporting is the mechanism by which an MTA publishes outcomes of certificate validation during outbound message delivery. Recipients of TLS reports learn whether certificates were valid, expired, or revoked, which helps identify security issues and misconfigurations in the sending MTA.

## Configuration

[Section titled “Configuration”](#configuration)

Stalwart [[mta/reports/analysis|automatically analyses]] TLS reports received from external hosts and can also generate aggregate reports to inform other hosts about the results of TLS validation. Outgoing TLS aggregate reports are configured on the [[ref/object/tls-report-settings|TlsReportSettings]] singleton (found in the WebUI under  Settings ›  MTA › Reports › TLS):

[[ref/object/tls-report-settings#fromname|`fromName`]]: expression returning the name used in the `From` header of the report. Default `'Report Subsystem'`.
[[ref/object/tls-report-settings#fromaddress|`fromAddress`]]: expression returning the email address used in the `From` header. Default `'noreply-tls@' + system('domain')`.
[[ref/object/tls-report-settings#subject|`subject`]]: expression returning the report subject. Default `'TLS Aggregate Report'`.
[[ref/object/tls-report-settings#orgname|`orgName`]]: expression returning the organisation name included in the report. Default `system('domain')`.
[[ref/object/tls-report-settings#contactinfo|`contactInfo`]]: optional expression returning contact information for the reporting organisation.
[[ref/object/tls-report-settings#sendfrequency|`sendFrequency`]]: expression returning the frequency at which aggregate reports are sent. Supported values are `hourly`, `daily`, `weekly`, and `never`. Default `daily`.
[[ref/object/tls-report-settings#maxreportsize|`maxReportSize`]]: expression returning the maximum report size in bytes. Default 5242880 (5 MB).
[[ref/object/tls-report-settings#dkimsigndomain|`dkimSignDomain`]]: expression returning the domain whose [[mta/authentication/dkim|DKIM]] signatures sign the outgoing report. Default `system('domain')`.

Example:

```
{  "fromName": {"else": "'TLS Report'"},  "fromAddress": {"else": "'[email protected]'"},  "subject": {"else": "'TLS Aggregate Report'"},  "orgName": {"else": "'The Foobar Organization'"},  "contactInfo": {"else": "'[email protected]'"},  "sendFrequency": {"else": "daily"},  "maxReportSize": {"else": "26214400"},  "dkimSignDomain": {"else": "'example.org'"}}
```

The submitter address for TLS reports is configured on the [[ref/object/report-settings|ReportSettings]] singleton via [[ref/object/report-settings#outboundreportsubmitter|`outboundReportSubmitter`]].