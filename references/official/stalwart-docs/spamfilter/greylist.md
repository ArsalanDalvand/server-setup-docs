---
title: "Greylisting"
source: "https://stalw.art/docs/spamfilter/greylist"
source_format: official-html-to-markdown
language: "en"
doc_slug: "spamfilter/greylist"
category: "spamfilter"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Greylisting"
---
# Greylisting

      

Greylisting temporarily rejects messages from unknown senders. When a message from an unfamiliar source arrives, the server returns a “try again later” response. Legitimate MTAs follow standard retry logic and will attempt the delivery again after a short delay, at which point the message is accepted. Many spam senders do not retry, so their messages never get through.

The decision to greylist is based on a triplet: the sender’s IP address, the sender’s email address, and the recipient’s email address. A given triplet is held in the greylist for a configurable duration.

## Advantages

[Section titled “Advantages”](#advantages)

Effective against bulk senders: greylisting stops senders that do not implement proper retry mechanisms, which captures a significant amount of spam.
Low overhead: unlike content-based filters, greylisting does not scan the body of each message and therefore consumes few resources.

## Challenges

[Section titled “Challenges”](#challenges)

Delivery delay: greylisting introduces a deliberate delay, potentially disrupting time-sensitive communications.
Adaptive senders: many bulk senders have adopted retry logic, reducing the efficacy of greylisting as a standalone technique.

## Configuration

[Section titled “Configuration”](#configuration)

Greylisting is configured through the [[ref/object/spam-settings#greylistfor|`greylistFor`]] field on the [[ref/object/spam-settings|SpamSettings]] singleton (found in the WebUI under  Settings ›  Spam Filter › General). The value is a duration that sets how long a triplet remains in the greylist; leaving the field unset disables greylisting. For example, setting `greylistFor` to `"30d"` keeps each triplet greylisted for thirty days.