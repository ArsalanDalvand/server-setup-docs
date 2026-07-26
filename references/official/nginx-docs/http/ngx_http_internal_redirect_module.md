---
title: "Module ngx_http_internal_redirect_module"
source: "https://nginx.org/en/docs/http/ngx_http_internal_redirect_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_internal_redirect_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_internal_redirect_module"
---
## Module ngx_http_internal_redirect_module

[Example Configuration](#example)
[Directives](#directives)
     [internal_redirect](#internal_redirect)

The `ngx_http_internal_redirect_module` module (1.23.4) allows
making an internal redirect.
In contrast to
[[http/ngx_http_rewrite_module.html|rewriting URIs]],
the redirection is made after checking
[[http/ngx_http_limit_req_module.html|request]] and
[[http/ngx_http_limit_conn_module.html|connection]] processing limits,
and [[http/ngx_http_access_module.html|access]] limits.

 
This module is available as part of our
[commercial subscription](https://www.f5.com/products/nginx).

#### Example Configuration

 

```nginx
limit_req_zone $jwt_claim_sub zone=jwt_sub:10m rate=1r/s;

server {
    location / {
        auth_jwt          "realm";
        auth_jwt_key_file key.jwk;

        internal_redirect @rate_limited;
    }

    location @rate_limited {
        internal;

        limit_req  zone=jwt_sub burst=10;
        proxy_pass http://backend;
    }
}
```

The example implements
[per-user](https://datatracker.ietf.org/doc/html/rfc7519#section-4.1.2)
[[http/ngx_http_limit_req_module.html|rate limiting]].
Implementation without [internal_redirect](#internal_redirect)
is vulnerable to DoS attacks by unsigned JWTs, as normally the
[[http/ngx_http_limit_req_module.html#limit_req|limit_req]]
check is performed
[[dev/development_guide.html#http_phases|before]]
[[http/ngx_http_auth_jwt_module.html#auth_jwt|auth_jwt]] check.
Using [internal_redirect](#internal_redirect)
allows reordering these checks.

#### Directives

            Syntax:
                

`internal_redirect uri;`

            Default:
                

            
            —
        
                

            Context:
                

`server`, `location`

Sets the URI for internal redirection of the request.
It is also possible to use a
[[http/ngx_http_core_module.html#location_named|named location]]
instead of the URI.
The `uri` value can contain variables.
If the `uri` value is empty,
then the redirect will not be made.