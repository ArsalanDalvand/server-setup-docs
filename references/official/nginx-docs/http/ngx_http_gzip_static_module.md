---
title: "Module ngx_http_gzip_static_module"
source: "https://nginx.org/en/docs/http/ngx_http_gzip_static_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_gzip_static_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_gzip_static_module"
---
## Module ngx_http_gzip_static_module

[Example Configuration](#example)
[Directives](#directives)
     [gzip_static](#gzip_static)

The `ngx_http_gzip_static_module` module allows
sending precompressed files with the “`.gz`”
filename extension instead of regular files.

This module is not built by default, it should be enabled with the
`--with-http_gzip_static_module`
configuration parameter.

#### Example Configuration

 

```nginx
gzip_static  on;
gzip_proxied expired no-cache no-store private auth;
```

#### Directives

            Syntax:
                

`gzip_static
on |
    off |
    always;`

            Default:
                

```nginx
gzip_static off;
```

            Context:
                

`http`, `server`, `location`

Enables (“`on`”) or disables (“`off`”)
checking the existence of precompressed files.
The following directives are also taken into account:
[[http/ngx_http_gzip_module.html#gzip_http_version|gzip_http_version]],
[[http/ngx_http_gzip_module.html#gzip_proxied|gzip_proxied]],
[[http/ngx_http_gzip_module.html#gzip_disable|gzip_disable]],
and [[http/ngx_http_gzip_module.html#gzip_vary|gzip_vary]].

With the “`always`” value (1.3.6), gzipped file is used
in all cases, without checking if the client supports it.
It is useful if there are no uncompressed files on the disk anyway
or the [[http/ngx_http_gunzip_module.html|ngx_http_gunzip_module]]
is used.

The files can be compressed using the `gzip` command,
or any other compatible one.
It is recommended that the modification date and time of original and
compressed files be the same.