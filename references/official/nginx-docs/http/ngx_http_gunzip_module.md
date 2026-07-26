---
title: "Module ngx_http_gunzip_module"
source: "https://nginx.org/en/docs/http/ngx_http_gunzip_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_gunzip_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_gunzip_module"
---
## Module ngx_http_gunzip_module

[Example Configuration](#example)
[Directives](#directives)
     [gunzip](#gunzip)
     [gunzip_buffers](#gunzip_buffers)

The `ngx_http_gunzip_module` module is a filter that
decompresses responses with “`Content-Encoding: gzip`”
for clients that do not support “gzip” encoding method.
The module will be useful when it is desirable to store
data compressed to save space and reduce I/O costs.

This module is not built by default, it should be enabled with the
`--with-http_gunzip_module`
configuration parameter.

#### Example Configuration

 

```nginx
location /storage/ {
    gunzip on;
    ...
}
```

#### Directives

            Syntax:
                

`gunzip on | off;`

            Default:
                

```nginx
gunzip off;
```

            Context:
                

`http`, `server`, `location`

Enables or disables decompression of gzipped responses
for clients that lack gzip support.
If enabled, the following directives are also taken into account
when determining if clients support gzip:
[[http/ngx_http_gzip_module.html#gzip_http_version|gzip_http_version]],
[[http/ngx_http_gzip_module.html#gzip_proxied|gzip_proxied]], and
[[http/ngx_http_gzip_module.html#gzip_disable|gzip_disable]].
See also the [[http/ngx_http_gzip_module.html#gzip_vary|gzip_vary]] directive.

            Syntax:
                

`gunzip_buffers number size;`

            Default:
                

```nginx
gunzip_buffers 32 4k|16 8k;
```

            Context:
                

`http`, `server`, `location`

Sets the `number` and `size` of buffers
used to decompress a response.
By default, the buffer size is equal to one memory page.
This is either 4K or 8K, depending on a platform.