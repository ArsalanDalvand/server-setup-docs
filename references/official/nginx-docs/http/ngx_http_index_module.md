---
title: "Module ngx_http_index_module"
source: "https://nginx.org/en/docs/http/ngx_http_index_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_index_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_index_module"
---
## Module ngx_http_index_module

[Example Configuration](#example)
[Directives](#directives)
     [index](#index)

The `ngx_http_index_module` module processes requests
ending with the slash character (‘`/`’).
Such requests can also be processed by the
[[http/ngx_http_autoindex_module.html|ngx_http_autoindex_module]]
and
[[http/ngx_http_random_index_module.html|ngx_http_random_index_module]]
modules.

#### Example Configuration

 

```nginx
location / {
    index index.$geo.html index.html;
}
```

#### Directives

            Syntax:
                

`index file ...;`

            Default:
                

```nginx
index index.html;
```

            Context:
                

`http`, `server`, `location`

Defines files that will be used as an index.
The `file` name can contain variables.
Files are checked in the specified order.
The last element of the list can be a file with an absolute path.
Example:

 

```nginx
index index.$geo.html index.0.html /index.html;
```

It should be noted that using an index file causes an internal redirect,
and the request can be processed in a different location.
For example, with the following configuration:

 

```nginx
location = / {
    index index.html;
}

location / {
    ...
}
```

a “`/`” request will actually be processed in the
second location as “`/index.html`”.