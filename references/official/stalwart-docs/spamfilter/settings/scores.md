---
title: "Scores"
source: "https://stalw.art/docs/spamfilter/settings/scores"
source_format: official-html-to-markdown
language: "en"
doc_slug: "spamfilter/settings/scores"
category: "spamfilter"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Scores"
---
# Scores

      

As described in the [[spamfilter#tags-and-scores|tags and scores]] section, the spam filter analyses each incoming message and produces a set of tags. Each tag contributes a score (positive values push the message toward spam, negative values toward ham), or triggers a direct discard or reject action. The cumulative score determines the final classification.

Tag-to-score mappings are configured through the [[ref/object/spam-tag|SpamTag]] object (found in the WebUI under  Settings ›  Spam Filter › Scores). SpamTag is a multi-variant object: the variant selects what happens when the tag fires on a message.

`Score`: adds a numeric score to the cumulative total. Carries a [[ref/object/spam-tag#tag|`tag`]] name and a [[ref/object/spam-tag#score|`score`]] value (positive values raise the spam score, negative values reduce it).
`Discard`: discards the message outright (no delivery, no bounce). Carries a [[ref/object/spam-tag#tag|`tag`]] name only.
`Reject`: rejects the message at SMTP time, causing the sending server to receive a delivery failure. Carries a [[ref/object/spam-tag#tag|`tag`]] name only.

## Examples

[Section titled “Examples”](#examples)

Assign a negative score to the classifier’s “likely ham” medium-confidence output:

```
{  "@type": "Score",  "tag": "PROB_HAM_MEDIUM",  "score": -3.0}
```

Assign a high positive score when the sender is on the Spamhaus DROP list:

```
{  "@type": "Score",  "tag": "RBL_SPAMHAUS_DROP",  "score": 7.0}
```

Discard any message that lands on a configured spam trap:

```
{  "@type": "Discard",  "tag": "SPAM_TRAP"}
```