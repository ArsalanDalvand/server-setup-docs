---
title: "Module ngx_http_mirror_module"
source: "https://nginx.org/en/docs/http/ngx_http_mirror_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_mirror_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_mirror_module"
---
## Module ngx_http_mirror_module

[Example Configuration](#example)
[Directives](#directives)
     [mirror](#mirror)
     [mirror_request_body](#mirror_request_body)

The `ngx_http_mirror_module` module (1.13.4) implements
mirroring of an original request
by creating background mirror subrequests.
Responses to mirror subrequests are ignored.

#### Example Configuration

 

```nginx
location / {
    mirror /mirror;
    proxy_pass http://backend;
}

location = /mirror {
    internal;
    proxy_pass http://test_backend$request_uri;
}
```

#### Directives

            Syntax:
                

`mirror uri | off;`

            Default:
                

```nginx
mirror off;
```

            Context:
                

`http`, `server`, `location`

Sets the URI to which an original request will be mirrored.
Several mirrors can be specified on the same configuration level.

            Syntax:
                

`mirror_request_body on | off;`

            Default:
                

```nginx
mirror_request_body on;
```

            Context:
                

`http`, `server`, `location`

Indicates whether the client request body is mirrored.
When enabled, the client request body will be read
prior to creating mirror subrequests.
In this case, unbuffered client request body proxying
set by the
[[http/ngx_http_proxy_module.html#proxy_request_buffering|proxy_request_buffering]],
[[http/ngx_http_fastcgi_module.html#fastcgi_request_buffering|fastcgi_request_buffering]],
[[http/ngx_http_scgi_module.html#scgi_request_buffering|scgi_request_buffering]],
and
[[http/ngx_http_uwsgi_module.html#uwsgi_request_buffering|uwsgi_request_buffering]]
directives will be disabled.

 

```nginx
location / {
    mirror /mirror;
    mirror_request_body off;
    proxy_pass http://backend;
}

location = /mirror {
    internal;
    proxy_pass http://log_backend;
    proxy_pass_request_body off;
    proxy_set_header Content-Length "";
    proxy_set_header X-Original-URI $request_uri;
}
```