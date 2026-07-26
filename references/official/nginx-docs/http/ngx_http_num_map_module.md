---
title: "Module ngx_http_num_map_module"
source: "https://nginx.org/en/docs/http/ngx_http_num_map_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_num_map_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_num_map_module"
---
## Module ngx_http_num_map_module

[Example Configuration](#example)
[Directives](#directives)
     [num_map](#num_map)

The `ngx_http_num_map_module` module (1.29.3)
creates variables whose values depend on numeric values
or numeric value ranges.

 
This module is available as part of our
[commercial subscription](https://www.f5.com/products/nginx).

#### Example Configuration

 

```nginx
num_map $remote_port $port_allow {
    default    0;

    80         1;
    443        1;
    <=1023     0;
    8080-8090  1;
}
```

#### Directives

            Syntax:
                

`num_map 
    [$number]
    $variable { ... }`

            Default:
                

            
            —
        
                

            Context:
                

`http`

Describes how the values of the specified variable depend on numeric values
or numeric value ranges.

 
Since variables are evaluated only when used, the mere existence
of even a large number of declared “`num_map`” variables
does not cause any extra costs for request processing.

Parameters inside the `num_map` block specify a mapping
between source and resulting values.

Source values are specified as numbers or as numeric ranges.

The following special parameters are also supported:

 
`default`

sets the resulting value if the source value matches none
of the specified variants.
When `default` is not specified, the default
resulting value will be an empty string.

`include`

includes a file with values.
There can be several inclusions.

`volatile`

indicates that the variable is not cacheable.