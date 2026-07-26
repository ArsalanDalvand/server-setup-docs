---
title: "Module ngx_http_slice_module"
source: "https://nginx.org/en/docs/http/ngx_http_slice_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_slice_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_slice_module"
---
## Module ngx_http_slice_module

[Known Issues](#issues)
[Example Configuration](#example)
[Directives](#directives)
     [slice](#slice)
[Embedded Variables](#variables)

The `ngx_http_slice_module` module (1.9.8) is a filter
that splits a request into subrequests,
each returning a certain range of response.
The filter provides more effective caching of big responses.

This module is not built by default, it should be enabled with the
`--with-http_slice_module`
configuration parameter.

#### Known Issues

Currently, the module does not work as expected in subrequests such as
[[http/ngx_http_proxy_module.html#proxy_cache_background_update|background cache update]].
In this case, a request is constructed without byte-range support.

#### Example Configuration

 

```nginx
location / {
    slice             1m;
    proxy_cache       cache;
    proxy_cache_key   $uri$is_args$args$slice_range;
    proxy_set_header  Range $slice_range;
    proxy_cache_valid 200 206 1h;
    proxy_pass        http://localhost:8000;
}
```

In this example,
the response is split into 1-megabyte cacheable slices.

#### Directives

            Syntax:
                

`slice size;`

            Default:
                

```nginx
slice 0;
```

            Context:
                

`http`, `server`, `location`

Sets the `size` of the slice.
The zero value disables splitting responses into slices.
Note that a too low value may result in excessive memory usage
and opening a large number of files.

In order for a subrequest to return the required range,
the `$slice_range` variable should be
[[http/ngx_http_proxy_module.html#proxy_set_header|passed]] to
the proxied server as the `Range` request header field.
If
[[http/ngx_http_proxy_module.html#proxy_cache|caching]]
is enabled, `$slice_range` should be added to the
[[http/ngx_http_proxy_module.html#proxy_cache_key|cache key]]
and caching of responses with 206 status code should be
[[http/ngx_http_proxy_module.html#proxy_cache_valid|enabled]].

#### Embedded Variables

The `ngx_http_slice_module` module
supports the following embedded variables:

 
`$slice_range`

the current slice range in
[HTTP
byte range](https://datatracker.ietf.org/doc/html/rfc7233#section-2.1) format,
for example, `bytes=0-1048575`.