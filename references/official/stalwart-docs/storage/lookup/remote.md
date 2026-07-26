---
title: "Remote"
source: "https://stalw.art/docs/storage/lookup/remote"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/lookup/remote"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Remote"
---
# Remote

      

Remote lookup lists retrieve key-value pairs from an external source over HTTP, allowing lookup data to be managed centrally and refreshed periodically. Two formats are supported: a simple list format where each entry occupies a single line, and CSV for more structured data.

Each list is associated with a refresh interval and a retry interval. Once the refresh interval elapses, the server automatically re-fetches the list so that values remain current without manual intervention.

## Configuration

[Section titled “Configuration”](#configuration)

Remote lookup lists are defined through the [[ref/object/http-lookup|HttpLookup]] object (found in the WebUI under  Settings ›  Lookups › HTTP Lists). Each record has the following fields:

[[ref/object/http-lookup#url|`url`]]: HTTP endpoint from which the list is fetched (required).
[[ref/object/http-lookup#enable|`enable`]]: when `true`, the list is active. Default: `true`.
[[ref/object/http-lookup#format|`format`]]: format of the remote list (required). Two variants are supported: `List` (one entry per line) and `Csv` (comma-separated values with configurable parsing).
[[ref/object/http-lookup#isgzipped|`isGzipped`]]: when `true`, the payload is decompressed with gzip before parsing. Default: `false`.
[[ref/object/http-lookup#refresh|`refresh`]]: how often to re-fetch the list. Default: `"12h"`.
[[ref/object/http-lookup#retry|`retry`]]: how long to wait before retrying a failed fetch. Default: `"1h"`.
[[ref/object/http-lookup#timeout|`timeout`]]: maximum time allowed for the fetch operation. Default: `"30s"`.
[[ref/object/http-lookup#namespace|`namespace`]]: the namespace under which the list’s entries are exposed to lookup functions (read-only; derived from the record identifier).

### CSV parsing options

[Section titled “CSV parsing options”](#csv-parsing-options)

When [[ref/object/http-lookup#format|`format`]] is set to the `Csv` variant, the variant carries additional fields:

[[ref/object/http-lookup#separator|`separator`]]: the delimiter between columns. Default: `","`.
[[ref/object/http-lookup#indexkey|`indexKey`]]: zero-based column index used as the lookup key. Default: `0`.
[[ref/object/http-lookup#indexvalue|`indexValue`]]: optional zero-based column index used as the lookup value.
[[ref/object/http-lookup#skipfirst|`skipFirst`]]: when `true`, the first line is treated as a header and skipped. Default: `false`.

### Resource limits

[Section titled “Resource limits”](#resource-limits)

The size of the retrieved list and each of its entries is bounded by:

[[ref/object/http-lookup#maxsize|`maxSize`]]: maximum total size of the list in bytes. Default: `"100mb"`.
[[ref/object/http-lookup#maxentries|`maxEntries`]]: maximum number of entries retained. Default: `100000`.
[[ref/object/http-lookup#maxentrysize|`maxEntrySize`]]: maximum size of a single entry in bytes. Default: `512`.

The list is truncated if it exceeds any of these limits.