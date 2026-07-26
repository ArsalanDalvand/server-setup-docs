---
title: "Module ngx_http_f4f_module"
source: "https://nginx.org/en/docs/http/ngx_http_f4f_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_f4f_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_f4f_module"
---
## Module ngx_http_f4f_module

[Example Configuration](#example)
[Directives](#directives)
     [f4f](#f4f)
     [f4f_buffer_size](#f4f_buffer_size)

The `ngx_http_f4f_module` module provides
server-side support for Adobe HTTP Dynamic Streaming (HDS).

This module implements handling of HTTP Dynamic Streaming requests in the
“`/videoSeg1-Frag1`” form — extracting the needed fragment
from the `videoSeg1.f4f` file using the `videoSeg1.f4x`
index file.
This module is an alternative to the Adobe’s f4f module (HTTP Origin Module)
for Apache.

Usual pre-processing with Adobe’s f4fpackager is required, see relevant
documentation for details.

 
This module is available as part of our
[commercial subscription](https://www.f5.com/products/nginx).

#### Example Configuration

 

```nginx
location /video/ {
    f4f;
    ...
}
```

#### Directives

            Syntax:
                

`f4f;`

            Default:
                

            
            —
        
                

            Context:
                

`location`

Turns on module processing in the surrounding location.

            Syntax:
                

`f4f_buffer_size size;`

            Default:
                

```nginx
f4f_buffer_size 512k;
```

            Context:
                

`http`, `server`, `location`

Sets the `size` of the buffer used for
reading the `.f4x` index file.