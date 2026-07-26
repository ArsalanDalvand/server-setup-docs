---
title: "Module ngx_http_access_module"
source: "https://nginx.org/en/docs/http/ngx_http_access_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_access_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_access_module"
---
## Module ngx_http_access_module

[Example Configuration](#example)
[Directives](#directives)
     [allow](#allow)
     [deny](#deny)

The `ngx_http_access_module` module allows
limiting access to certain client addresses.

Access can also be limited by
[[http/ngx_http_auth_basic_module.html|password]], by the
[[http/ngx_http_auth_request_module.html|result of subrequest]],
or by [[http/ngx_http_auth_jwt_module.html|JWT]].
Simultaneous limitation of access by address and by password is controlled
by the [[http/ngx_http_core_module.html#satisfy|satisfy]] directive.

#### Example Configuration

 

```nginx
location / {
    deny  192.168.1.1;
    allow 192.168.1.0/24;
    allow 10.1.1.0/16;
    allow 2001:0db8::/32;
    deny  all;
}
```

The rules are checked in sequence until the first match is found.
In this example, access is allowed only for IPv4 networks
`10.1.1.0/16` and `192.168.1.0/24`
excluding the address `192.168.1.1`,
and for IPv6 network `2001:0db8::/32`.
In case of a lot of rules, the use of the
[[http/ngx_http_geo_module.html|ngx_http_geo_module]]
module variables is preferable.

#### Directives

            Syntax:
                

`allow
address |
    CIDR |
    unix: |
    all;`

            Default:
                

            
            —
        
                

            Context:
                

`http`, `server`, `location`, `limit_except`

Allows access for the specified network or address.
If the special value `unix:` is specified (1.5.1),
allows access for all UNIX-domain sockets.

Several `allow` directives can be specified on the same level.
These directives are inherited from the previous configuration level
if and only if there are no `allow` and
`deny` directives defined on the current level.

            Syntax:
                

`deny
address |
    CIDR |
    unix: |
    all;`

            Default:
                

            
            —
        
                

            Context:
                

`http`, `server`, `location`, `limit_except`

Denies access for the specified network or address.
If the special value `unix:` is specified (1.5.1),
denies access for all UNIX-domain sockets.

Several `deny` directives can be specified on the same level.
These directives are inherited from the previous configuration level
if and only if there are no `allow` and
`deny` directives defined on the current level.