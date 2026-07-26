---
title: "ASN and GeoIP"
source: "https://stalw.art/docs/server/asn"
source_format: official-html-to-markdown
language: "en"
doc_slug: "server/asn"
category: "server"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "ASN and GeoIP"
---
# ASN and GeoIP

      

An Autonomous System Number (ASN) identifies the network operator responsible for a range of IP addresses. GeoIP data maps IP addresses to a country, region, or city. Together, these sources describe who operates an address and where it is located, allowing incoming connections to be classified by network and geography.

Stalwart uses ASN and GeoIP information in several places. The [[spamfilter|spam filter]] tracks reputation metrics by IP, sender, domain, and ASN, and the spam classifier incorporates the same data during training. When processing messages, the server includes ASN and GeoIP details in the `Received` header so that message paths can be audited after the fact.

Two data-loading strategies are supported. The preferred approach downloads CSV files from HTTP resources, which keeps ASN-to-IP and country-to-IP mappings current. A DNS-based alternative queries TXT records exposed by services such as Team Cymru; this mode is useful where outbound HTTP is restricted but carries slightly less detail.

## Configuration

[Section titled “Configuration”](#configuration)

ASN and GeoIP data sources are configured through the [[ref/object/asn|Asn]] singleton (found in the WebUI under  Settings ›  Network › ASN & GeoIP). The object is multi-variant: each instance selects one of `Disabled`, `Resource`, or `Dns`, and the chosen variant determines which fields apply.

### HTTP resources

[Section titled “HTTP resources”](#http-resources)

The `Resource` variant downloads CSV data files over HTTP. It exposes the following fields:

[[ref/object/asn#asnurls|`asnUrls`]]: URLs of the CSV files that contain IP-to-ASN mappings.
[[ref/object/asn#geourls|`geoUrls`]]: URLs of the CSV files that contain IP-to-country mappings.
[[ref/object/asn#expires|`expires`]]: refresh interval for downloaded data. Default `"1d"`.
[[ref/object/asn#timeout|`timeout`]]: maximum time to wait for a fetch. Default `"5m"`.
[[ref/object/asn#maxsize|`maxSize`]]: maximum size accepted for a downloaded file. Default `"100mb"`.
[[ref/object/asn#httpauth|`httpAuth`]]: HTTP authentication method (`Unauthenticated`, `Basic`, or `Bearer`).
[[ref/object/asn#httpheaders|`httpHeaders`]]: additional HTTP headers sent with each request.

For example:

```
{  "@type": "Resource",  "expires": "1d",  "timeout": "5m",  "maxSize": "100mb",  "asnUrls": [    "https://cdn.jsdelivr.net/npm/@ip-location-db/asn/asn-ipv4.csv",    "https://cdn.jsdelivr.net/npm/@ip-location-db/asn/asn-ipv6.csv"  ],  "geoUrls": [    "https://cdn.jsdelivr.net/npm/@ip-location-db/geolite2-geo-whois-asn-country/geolite2-geo-whois-asn-country-ipv4.csv",    "https://cdn.jsdelivr.net/npm/@ip-location-db/geolite2-geo-whois-asn-country/geolite2-geo-whois-asn-country-ipv6.csv"  ],  "httpAuth": {"@type": "Unauthenticated"}}
```

### DNS queries

[Section titled “DNS queries”](#dns-queries)

The `Dns` variant queries TXT records from a zone that exposes ASN and geolocation data (for example Team Cymru’s `origin.asn.cymru.com` and `origin6.asn.cymru.com`). Its fields are:

[[ref/object/asn#zoneipv4|`zoneIpV4`]]: DNS zone queried for IPv4 addresses.
[[ref/object/asn#zoneipv6|`zoneIpV6`]]: DNS zone queried for IPv6 addresses.
[[ref/object/asn#separator|`separator`]]: character that separates fields in the TXT record. Default `"|"`.
[[ref/object/asn#indexasn|`indexAsn`]]: zero-based position of the ASN field. Default `0`.
[[ref/object/asn#indexasnname|`indexAsnName`]]: position of the ASN name field.
[[ref/object/asn#indexcountry|`indexCountry`]]: position of the country-code field.

For example:

```
{  "@type": "Dns",  "separator": "|",  "zoneIpV4": "origin.asn.cymru.com",  "zoneIpV6": "origin6.asn.cymru.com",  "indexAsn": 0,  "indexAsnName": 1,  "indexCountry": 2}
```

### Disabling lookups

[Section titled “Disabling lookups”](#disabling-lookups)

Selecting the `Disabled` variant turns off both ASN and GeoIP resolution.