---
title: "Caching"
source: "https://stalw.art/docs/server/cache"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/cache"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Caching"
---
# Caching

      

Stalwart keeps a set of in-memory caches in front of slower backends (the data store, the DNS resolver, the authentication subsystem). Caching frequently accessed records reduces latency and takes load off the underlying backends without changing their consistency guarantees.

The eviction policy is a modified Clock-PRO algorithm, close in spirit to the later S3-FIFO scheme. It is scan-resistant, so a transient burst of unrelated lookups does not evict hot entries, and it reaches hit rates comparable to LRU and W-TinyLFU on realistic workloads.

## Memory usage

[Section titled “Memory usage”](#memory-usage)

Cache sizes should be matched to the workload. On hosts with ample memory and many users, larger caches reduce evictions and improve hit rates. On hosts with limited memory, smaller caches conserve RAM at the cost of some hit rate; monitoring resident set size and the cache-hit metrics published by the server is the normal way to pick an appropriate balance.

## Configuration

[Section titled “Configuration”](#configuration)

Every cache is configured through the [[ref/object/cache|Cache]] singleton (found in the WebUI under  Settings ›  Storage › Cache). Each field is a byte-size value with a suffix (for example `"50mb"`); minimum size is `2048` bytes.

### Data caches

[Section titled “Data caches”](#data-caches)

[[ref/object/cache#messages|`messages`]]: email account data including message UIDs, flags, and mailbox metadata. Default `"50mb"`.
[[ref/object/cache#events|`events`]]: calendar events, including start / end times and timezone data. Default `"10mb"`.
[[ref/object/cache#scheduling|`scheduling`]]: calendar scheduling state. Default `"1mb"`.
[[ref/object/cache#contacts|`contacts`]]: address-book and contact data. Default `"10mb"`.
[[ref/object/cache#files|`files`]]: file-storage metadata such as paths and permissions. Default `"10mb"`.
[[ref/object/cache#mailinglists|`mailingLists`]]: mailing-list metadata. Default `"2mb"`.
[[ref/object/cache#dkimsignatures|`dkimSignatures`]]: parsed DKIM signing configurations. Default `"10mb"`.

### Directory caches

[Section titled “Directory caches”](#directory-caches)

[[ref/object/cache#accounts|`accounts`]]: resolved account records. Default `"20mb"`.
[[ref/object/cache#domains|`domains`]]: domain records. Default `"5mb"`.
[[ref/object/cache#domainnames|`domainNames`]]: positive domain-name lookup results. Default `"10mb"`.
[[ref/object/cache#domainnamesnegative|`domainNamesNegative`]]: negative (not-found) domain-name lookup results. Default `"1mb"`.
[[ref/object/cache#emailaddresses|`emailAddresses`]]: positive email-address lookup results. Default `"10mb"`.
[[ref/object/cache#emailaddressesnegative|`emailAddressesNegative`]]: negative email-address lookup results. Default `"2mb"`.
[[ref/object/cache#tenants|`tenants`]]: tenant records. Default `"5mb"`.
[[ref/object/cache#negativettl|`negativeTtl`]]: how long to keep negative domain and account lookup entries. Default `"1h"`.

### Security caches

[Section titled “Security caches”](#security-caches)

[[ref/object/cache#accesstokens|`accessTokens`]]: resolved access tokens, reducing revalidation during authentication. Default `"10mb"`.
[[ref/object/cache#httpauth|`httpAuth`]]: short-lived state for HTTP authentication. Default `"1mb"`.

### DNS caches

[Section titled “DNS caches”](#dns-caches)

[[ref/object/cache#dnstxt|`dnsTxt`]]: TXT records used for SPF, DKIM, and DMARC. Default `"5mb"`.
[[ref/object/cache#dnsmx|`dnsMx`]]: MX records used for outbound routing. Default `"5mb"`.
[[ref/object/cache#dnsptr|`dnsPtr`]]: reverse (PTR) lookup results. Default `"1mb"`.
[[ref/object/cache#dnsipv4|`dnsIpv4`]]: forward IPv4 (A) lookup results. Default `"5mb"`.
[[ref/object/cache#dnsipv6|`dnsIpv6`]]: forward IPv6 (AAAA) lookup results. Default `"5mb"`.
[[ref/object/cache#dnstlsa|`dnsTlsa`]]: TLSA records used for DANE validation. Default `"1mb"`.
[[ref/object/cache#dnsmtasts|`dnsMtaSts`]]: MTA-STS policy records. Default `"1mb"`.
[[ref/object/cache#dnsrbl|`dnsRbl`]]: DNSBL / RBL lookup results. Default `"5mb"`.