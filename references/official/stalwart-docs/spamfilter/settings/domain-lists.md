---
title: "Domain lists"
source: "https://stalw.art/docs/spamfilter/settings/domain-lists"
source_format: official-html-to-markdown
language: "en"
doc_slug: "spamfilter/settings/domain-lists"
category: "spamfilter"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Domain lists"
---
# Domain lists

      

Several of the lookup lists used by the spam filter categorise domains according to their behaviour. These lists inform rule evaluation and allow the filter to react differently to messages involving specific kinds of domain.

The entries in each list are stored as [[ref/object/memory-lookup-key|MemoryLookupKey]] objects (found in the WebUI under  Settings ›  Lookups › In-Memory Keys,  Settings ›  Spam Filter › Lists › Blocked Domains,  Settings ›  Spam Filter › Lists › Spam Traps,  Settings ›  Spam Filter › Lists › Trusted Domains,  Settings ›  Spam Filter › Lists › URL Redirectors). The [[ref/object/memory-lookup-key#namespace|`namespace`]] field selects which list the entry belongs to, and the [[ref/object/memory-lookup-key#key|`key`]] field carries the domain itself.

## Trusted domains

[Section titled “Trusted domains”](#trusted-domains)

Entries in the `trusted-domains` namespace list domain names considered trustworthy. Messages associated with these domains bypass DNS block-list checks.

## Disposable domains

[Section titled “Disposable domains”](#disposable-domains)

Entries in the `disposable-providers` namespace identify domains associated with providers offering disposable email addresses. Such addresses are typically used for temporary communication and can be discarded after use. Messages from these domains do not directly affect the spam score, but they can shift the score when combined with other rule results.

## Free domains

[Section titled “Free domains”](#free-domains)

Entries in the `freemail-providers` namespace list domain names associated with providers that offer free email accounts, such as popular webmail services. Messages from these domains do not directly alter the spam score, but the filter can adjust it when these domains appear in combination with other signals. For example, a message whose `From` address uses one free-mail provider and whose `Reply-To` uses another may be flagged as suspicious.

## URL redirectors

[Section titled “URL redirectors”](#url-redirectors)

Entries in the `url-redirectors` namespace list domain names that belong to URL redirector services (for example, bit.ly). When the spam filter encounters a URL from one of these domains inside a message body, it follows the redirect to determine the final destination. Nested redirects are also followed, so that multi-layered chains are fully resolved before further analysis.