---
title: "Spam filter"
source: "https://stalw.art/docs/mta/filter/spam"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/filter/spam"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Spam filter"
---
# Spam filter

      

Spam filtering is an essential component of any modern mail server, designed to identify and segregate unsolicited or potentially harmful messages from the legitimate mail flow. Unsolicited messages (spam) clutter mailboxes and can pose significant security risks; efficient spam filtering ensures that genuine messages reach their recipients while unwanted or malicious ones are quarantined or discarded.

Stalwart includes a built-in spam and phishing filter implemented as a collection of Sieve scripts. Administrators can modify or extend these scripts to suit their deployment.

See the [[spamfilter|Spam and Phishing filter]] section for details on the filter’s rules, training, and tuning.