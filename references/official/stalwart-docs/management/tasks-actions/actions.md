---
title: "Actions"
source: "https://stalw.art/docs/management/tasks-actions/actions"
source_format: official-html-to-markdown
language: "en"
doc_slug: "management/tasks-actions/actions"
category: "management"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Actions"
---
# Actions

      

An Action is a server management operation triggered on demand. Unlike a scheduled [[management/tasks-actions/tasks|Task]], an Action runs immediately in response to an administrator request: reloading configuration, flushing caches, pausing the outbound queue, running a troubleshooting probe, or classifying a message through the spam filter. Each invocation is represented as an [[ref/object/action|Action]] object (found in the WebUI under  Management ›  Actions) of one of the variants described below.

Actions can be created from the [[management/webui|WebUI]], through the [[management/cli|CLI]] using `stalwart-cli create action/<variant-name>`, or over the JMAP API through the `x:Action/set` method on the [[ref/object/action|Action]] object. The server records each Action so that results, such as the spam classification score or a DMARC troubleshooting report, can be retrieved afterwards with `x:Action/get` or `stalwart-cli get action <id>`.

## Variants

[Section titled “Variants”](#variants)

Variants that require input fields, such as a message body or a remote IP, carry those fields alongside the variant identifier; the full schema for each is on the reference page.

### Reload settings

[Section titled “Reload settings”](#reload-settings)

The `ReloadSettings` variant rereads the server settings from the data store without restarting the process. No input fields are required.

### Reload TLS certificates

[Section titled “Reload TLS certificates”](#reload-tls-certificates)

The `ReloadTlsCertificates` variant reloads TLS certificates from disk or from the configured ACME store so that renewals take effect without a restart. No input fields are required.

### Reload lookup stores

[Section titled “Reload lookup stores”](#reload-lookup-stores)

The `ReloadLookupStores` variant refreshes the lookup stores backing access lists, blocklists, and similar in-memory datasets. No input fields are required.

### Reload blocked IPs

[Section titled “Reload blocked IPs”](#reload-blocked-ips)

The `ReloadBlockedIps` variant reloads the blocked IP list applied by the inbound filters. No input fields are required.

### Update applications

[Section titled “Update applications”](#update-applications)

The `UpdateApps` variant installs pending updates for the hosted web [[ref/object/application|Applications]], such as the WebUI bundle. No input fields are required. Spam-filter rule updates are handled separately through the asynchronous `SpamFilterMaintenance` variant of [[ref/object/task|Task]].

### Troubleshoot DMARC

[Section titled “Troubleshoot DMARC”](#troubleshoot-dmarc)

The `TroubleshootDmarc` variant runs the same SPF, DKIM, ARC, and DMARC checks that the server applies to an incoming message. The caller supplies the [[ref/object/action#remoteip|`remoteIp`]], the [[ref/object/action#ehlodomain|`ehloDomain`]], the [[ref/object/action#mailfrom|`mailFrom`]] address, the [[ref/object/action#spfehlodomain|`spfEhloDomain`]] and [[ref/object/action#spfmailfromdomain|`spfMailFromDomain`]] used for the SPF checks, and an optional [[ref/object/action#message|`message`]] body. The server populates the per-check results (SPF, reverse DNS, DKIM, ARC, DMARC), the overall DKIM and DMARC pass flags, the applied [[ref/object/action#dmarcpolicy|`dmarcPolicy`]], and the [[ref/object/action#elapsed|`elapsed`]] time.

### Classify spam

[Section titled “Classify spam”](#classify-spam)

The `ClassifySpam` variant submits a raw message to the spam filter and returns the resulting score, tags, and overall classification. The caller supplies the raw [[ref/object/action#message|`message`]] together with the envelope context: [[ref/object/action#remoteip|`remoteIp`]], [[ref/object/action#ehlodomain|`ehloDomain`]], optional [[ref/object/action#authenticatedas|`authenticatedAs`]] identity, [[ref/object/action#istls|`isTls`]] flag, [[ref/object/action#envfrom|`envFrom`]] address with optional [[ref/object/action#envfromparameters|`envFromParameters`]], and the list of [[ref/object/action#envrcptto|`envRcptTo`]] recipients. The server populates the numeric [[ref/object/action#score|`score`]], the map of contributing [[ref/object/action#tags|`tags`]], and the final [[ref/object/action#result|`result`]]: spam, ham, reject, or discard.

### Invalidate caches

[Section titled “Invalidate caches”](#invalidate-caches)

The `InvalidateCaches` variant clears all in-memory caches. No input fields are required.

### Invalidate negative caches

[Section titled “Invalidate negative caches”](#invalidate-negative-caches)

The `InvalidateNegativeCaches` variant clears only negative cache entries, such as failed DNS lookups or missing-record results, leaving positive cache entries in place. No input fields are required.

### Pause the outbound queue

[Section titled “Pause the outbound queue”](#pause-the-outbound-queue)

The `PauseMtaQueue` variant pauses outbound MTA queue processing. Messages continue to accumulate but delivery attempts stop until resumed. No input fields are required.

### Resume the outbound queue

[Section titled “Resume the outbound queue”](#resume-the-outbound-queue)

The `ResumeMtaQueue` variant resumes outbound MTA queue processing after a pause. No input fields are required.

## Permissions

[Section titled “Permissions”](#permissions)

Creating, reading, querying, and destroying Actions are gated by the `sysActionCreate`, `sysActionGet`, `sysActionQuery`, and `sysActionDestroy` permissions respectively. Administrators granted these permissions can invoke any variant; finer-grained restrictions are expressed through the standard permission model.