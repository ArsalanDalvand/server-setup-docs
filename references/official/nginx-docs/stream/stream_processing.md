---
title: "How nginx processes a TCP/UDP session"
source: "https://nginx.org/en/docs/stream/stream_processing.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "stream/stream_processing"
category: "stream"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "How nginx processes a TCP/UDP session"
---
## How nginx processes a TCP/UDP session

A TCP/UDP session from a client is processed
in successive steps called phases:

 

`Post-accept`

The first phase after accepting a client connection.
The [[stream/ngx_stream_realip_module.html|ngx_stream_realip_module]]
module is invoked at this phase.

`Pre-access`

Preliminary check for access.
The
[[stream/ngx_stream_limit_conn_module.html|ngx_stream_limit_conn_module]]
and
[[stream/ngx_stream_set_module.html|ngx_stream_set_module]]
modules are invoked at this phase.

`Access`

Client access limitation before actual data processing.
At this phase,
the [[stream/ngx_stream_access_module.html|ngx_stream_access_module]]
module is invoked,
for [njs](../njs/index.html),
the [[stream/ngx_stream_js_module.html#js_access|js_access]] directive
is invoked.

`SSL`

TLS/SSL termination.
The [[stream/ngx_stream_ssl_module.html|ngx_stream_ssl_module]]
module is invoked at this phase.

`Preread`

Reading initial bytes of data into the
[[stream/ngx_stream_core_module.html#preread_buffer_size|preread buffer]]
to allow modules such as
[[stream/ngx_stream_ssl_preread_module.html|ngx_stream_ssl_preread_module]]
analyze the data before its processing.
For [njs](../njs/index.html),
the [[stream/ngx_stream_js_module.html#js_preread|js_preread]] directive
is invoked at this phase.

`Content`

Mandatory phase where data is actually processed, usually
[[stream/ngx_stream_proxy_module.html|proxied]] to
[[stream/ngx_stream_upstream_module.html|upstream]] servers,
or a specified value
is [[stream/ngx_stream_return_module.html|returned]] to a client.
For [njs](../njs/index.html),
the [[stream/ngx_stream_js_module.html#js_filter|js_filter]] directive
is invoked at this phase.

`Log`

The final phase
where the result of a client session processing is recorded.
The [[stream/ngx_stream_log_module.html|ngx_stream_log_module]]
module is invoked at this phase.