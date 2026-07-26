---
title: "Module ngx_http_random_index_module"
source: "https://nginx.org/en/docs/http/ngx_http_random_index_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_random_index_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_random_index_module"
---
## Module ngx_http_random_index_module

[Example Configuration](#example)
[Directives](#directives)
     [random_index](#random_index)

The `ngx_http_random_index_module` module processes requests
ending with the slash character (‘`/`’) and picks a random
file in a directory to serve as an index file.
The module is processed before the
[[http/ngx_http_index_module.html|ngx_http_index_module]]
module.

This module is not built by default, it should be enabled with the
`--with-http_random_index_module`
configuration parameter.

#### Example Configuration

 

```nginx
location / {
    random_index on;
}
```

#### Directives

            Syntax:
                

`random_index on | off;`

            Default:
                

```nginx
random_index off;
```

            Context:
                

`location`

Enables or disables module processing in a surrounding location.