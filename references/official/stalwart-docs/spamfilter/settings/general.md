---
title: "Overview"
source: "https://stalw.art/docs/spamfilter/settings/general"
source_format: official-html-to-markdown
language: "en"
doc_slug: "spamfilter/settings/general"
category: "spamfilter"
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

      

The spam filter is enabled by default in Stalwart. The global on/off switch is the [[ref/object/spam-settings#enable|`enable`]] field on the [[ref/object/spam-settings|SpamSettings]] singleton (found in the WebUI under  Settings ›  Spam Filter › General). For per-session control, use [[ref/object/mta-stage-data#enablespamfilter|`enableSpamFilter`]] on [[ref/object/mta-stage-data|MtaStageData]], which accepts an expression and is evaluated at the [[mta/inbound/data#spam-filtering|SMTP DATA]] stage.

## Headers

[Section titled “Headers”](#headers)

The spam filter reports its analysis through two headers added to each delivered message: `X-Spam-Status` and `X-Spam-Result`.

### X-Spam-Status

[Section titled “X-Spam-Status”](#x-spam-status)

The `X-Spam-Status` header states whether the message was classified as spam, as a simple `Yes` or `No`, along with the final score. End users can build Sieve filters on top of this header to sort or quarantine suspected spam themselves.

### X-Spam-Result

[Section titled “X-Spam-Result”](#x-spam-result)

The `X-Spam-Result` header lists every tag applied by the filter and its associated score, making the filter’s decision auditable and easy to debug.

Both headers are added unconditionally under their fixed names.

## Thresholds

[Section titled “Thresholds”](#thresholds)

The threshold settings on [[ref/object/spam-settings|SpamSettings]] determine how aggressively the spam filter acts on incoming messages.

### Spam threshold

[Section titled “Spam threshold”](#spam-threshold)

[[ref/object/spam-settings#scorespam|`scoreSpam`]] sets the score at or above which a message is classified as spam. The default is `5`, so any message whose cumulative score exceeds 5 is marked as spam in the `X-Spam-Status` header.

### Discard threshold

[Section titled “Discard threshold”](#discard-threshold)

[[ref/object/spam-settings#scorediscard|`scoreDiscard`]] sets the score at or above which a message is discarded outright (dropped silently without delivery). The default value is `0`, which effectively leaves the feature inactive in most practical scoring setups. Raising this value activates the behaviour.

### Reject threshold

[Section titled “Reject threshold”](#reject-threshold)

[[ref/object/spam-settings#scorereject|`scoreReject`]] sets the score at or above which a message is rejected at SMTP time, causing the sending server to receive a delivery failure. The default value is `0`; raising it activates the behaviour.

## Address book integration

[Section titled “Address book integration”](#address-book-integration)

Stalwart’s spam filter integrates with the recipient’s [[collaboration/contact|address book]] to reduce false positives. When a message comes from a sender present in the recipient’s address book, it is treated as legitimate (ham) regardless of the score that would otherwise be assigned.

The feature is controlled by [[ref/object/spam-settings#trustcontacts|`trustContacts`]] on SpamSettings, which defaults to `true`. With this setting active, trusted contacts will not have their messages misclassified because of wording, formatting, or other typical spam indicators.

## Updates

[Section titled “Updates”](#updates)

On first startup, Stalwart downloads the default spam filter rules from the GitHub repository and stores them locally, so the filter is immediately ready for use. Administrators can then update the rule set on demand from `Maintenance` > `Update SPAM rules` in the [[management/webui|WebUI]].

### Resource location

[Section titled “Resource location”](#resource-location)

The URL from which spam filter rules are downloaded is set through [[ref/object/spam-settings#spamfilterrulesurl|`spamFilterRulesUrl`]] on SpamSettings. The default is `"https://github.com/stalwartlabs/spam-filter/releases/latest/download/spam-filter-rules.json.gz"`. A local `file://` URL can be used to serve rules from disk.

### Scheduled updates

[Section titled “Scheduled updates”](#scheduled-updates)

Rule refreshes are triggered by a [[ref/object/task|Task]] (found in the WebUI under  Management ›  Tasks › Scheduled,  Management ›  Tasks › Failed) of variant `SpamFilterMaintenance` with its operation set to `updateRules`. The task can be scheduled on demand by the administrator to re-download the rule set from [[ref/object/spam-settings#spamfilterrulesurl|`spamFilterRulesUrl`]]. There is no built-in automatic schedule at present.