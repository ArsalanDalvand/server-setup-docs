---
title: "Module ngx_http_split_clients_module"
source: "https://nginx.org/en/docs/http/ngx_http_split_clients_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_split_clients_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_split_clients_module"
---
## Module ngx_http_split_clients_module

[Example Configuration](#example)
[Directives](#directives)
     [split_clients](#split_clients)

The `ngx_http_split_clients_module` module creates
variables suitable for A/B testing, also known as split testing.

#### Example Configuration

 

```nginx
http {
    split_clients "${remote_addr}AAA" $variant {
                   0.5%               .one;
                   2.0%               .two;
                   *                  "";
    }

    server {
        location / {
            index index${variant}.html;
```

#### Directives

            Syntax:
                

`split_clients
string
$variable { ... }`

            Default:
                

            
            —
        
                

            Context:
                

`http`

Creates a variable for A/B testing, for example:

 

```nginx
split_clients "${remote_addr}AAA" $variant {
               0.5%               .one;
               2.0%               .two;
               *                  "";
}
```

The value of the original string is hashed using MurmurHash2.
In the example given, hash values from 0 to 21474835 (0.5%)
correspond to the
value `".one"` of the `$variant` variable,
hash values from 21474836 to 107374180 (2%) correspond to
the value `".two"`,
and hash values from 107374181 to 4294967295 correspond to
the value `""` (an empty string).