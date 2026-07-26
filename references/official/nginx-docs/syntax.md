---
title: "Configuration file measurement units"
source: "https://nginx.org/en/docs/syntax.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "syntax"
category: "nginx"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Configuration file measurement units"
---
## Configuration file measurement units

[Sizes and offsets](#size)
[Time intervals](#time)

nginx supports several measurement units for specifying
sizes, offsets, and time intervals within configuration files.

#### Sizes and offsets

Sizes can be specified in bytes, kilobytes, or megabytes
using the following suffixes:

 

`k` and `K` for kilobytes

`m` and `M` for megabytes

For example,
“`1024`”, “`8k`”, “`1m`”.

Offsets can be also specified in gigabytes using the
`g` or `G` suffixes.

#### Time intervals

Time intervals can be specified in milliseconds, seconds,
minutes, hours, days and so on, using the following suffixes:

msmilliseconds
sseconds (default)
mminutes
hhours
ddays
wweeks
Mmonths, 30 days
yyears, 365 days

Multiple units can be combined in a single value
by specifying them in the order from the most to the least significant,
and optionally separated by whitespace.
For example, “`1h 30m`” specifies the same time
as “`90m`” or “`5400s`”.

 

A value without a suffix means seconds.

It is recommended to always specify a suffix.

Certain time intervals can be specified only with a seconds resolution.