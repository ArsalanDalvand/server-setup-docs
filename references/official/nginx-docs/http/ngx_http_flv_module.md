---
title: "Module ngx_http_flv_module"
source: "https://nginx.org/en/docs/http/ngx_http_flv_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_flv_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_flv_module"
---
## Module ngx_http_flv_module

[Example Configuration](#example)
[Directives](#directives)
     [flv](#flv)

The `ngx_http_flv_module` module provides pseudo-streaming
server-side support for Flash Video (FLV) files.

It handles requests with the `start` argument in
the request URI’s query string specially, by sending back the contents
of a file starting from the requested byte offset and with the prepended FLV
header.

This module is not built by default, it should be enabled with the
`--with-http_flv_module`
configuration parameter.

#### Example Configuration

 

```nginx
location ~ \.flv$ {
    flv;
}
```

#### Directives

            Syntax:
                

`flv;`

            Default:
                

            
            —
        
                

            Context:
                

`location`

Turns on module processing in a surrounding location.