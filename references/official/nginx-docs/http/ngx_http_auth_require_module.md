---
title: "Module ngx_http_auth_require_module"
source: "https://nginx.org/en/docs/http/ngx_http_auth_require_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_auth_require_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_auth_require_module"
---
## Module ngx_http_auth_require_module

[Example Configuration](#example)
[Directives](#directives)
     [auth_require](#auth_require)

The `ngx_http_auth_require_module` module (1.29.0)
implements variable-based client authorization.
It can also use variables
provided by other access modules such as
[[http/ngx_http_auth_request_module.html|ngx_http_auth_request_module]]
or
[[http/ngx_http_auth_request_module.html|ngx_http_auth_oidc_module]].

 
This module is available as part of our
[commercial subscription](https://www.f5.com/products/nginx).

#### Example Configuration

 

```nginx
http {
    oidc_provider my_idp {
        ...
    }

    map $oidc_claim_role $admin_role {
        "admin" 1;
    }

    server {
        auth_oidc my_idp;

        location /admin {
            auth_require $admin_role;
        }
    }
}
```

#### Directives

            Syntax:
                

`auth_require
$value ...
    [error=4xx |
                              5xx]
;`

            Default:
                

```nginx
auth_require off;
```

            Context:
                

`http`, `server`, `location`, `limit_except`

Enables authorization based on the specified variables.
The access is allowed only if all the variables are not
empty and are not equal to “0”.
Otherwise, the module returns `403` code, which can be
overridden by the `error` parameter.
Several `auth_require` directives can be used to return
different error codes.