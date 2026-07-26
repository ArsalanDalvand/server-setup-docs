---
title: "Module ngx_http_empty_gif_module"
source: "https://nginx.org/en/docs/http/ngx_http_empty_gif_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_empty_gif_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_empty_gif_module"
---
## Module ngx_http_empty_gif_module

[Example Configuration](#example)
[Directives](#directives)
     [empty_gif](#empty_gif)

The `ngx_http_empty_gif_module` module emits
single-pixel transparent GIF.

#### Example Configuration

 

```nginx
location = /_.gif {
    empty_gif;
}
```

#### Directives

            Syntax:
                

`empty_gif;`

            Default:
                

            
            —
        
                

            Context:
                

`location`

Turns on module processing in a surrounding location.