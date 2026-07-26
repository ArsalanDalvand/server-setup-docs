---
title: "Module ngx_http_browser_module"
source: "https://nginx.org/en/docs/http/ngx_http_browser_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_browser_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_browser_module"
---
## Module ngx_http_browser_module

[Example Configuration](#example)
[Directives](#directives)
     [ancient_browser](#ancient_browser)
     [ancient_browser_value](#ancient_browser_value)
     [modern_browser](#modern_browser)
     [modern_browser_value](#modern_browser_value)

The `ngx_http_browser_module` module creates variables
whose values depend on the value of the “User-Agent”
request header field:

 
`$modern_browser`

equals the value set by the [modern_browser_value](#modern_browser_value) directive,
if a browser was identified as modern;

`$ancient_browser`

equals the value set by the [ancient_browser_value](#ancient_browser_value) directive,
if a browser was identified as ancient;

`$msie`

equals “1” if a browser was identified as MSIE of any version.

#### Example Configuration

Choosing an index file:

 

```nginx
modern_browser_value "modern.";

modern_browser msie      5.5;
modern_browser gecko     1.0.0;
modern_browser opera     9.0;
modern_browser safari    413;
modern_browser konqueror 3.0;

index index.${modern_browser}html index.html;
```

Redirection for old browsers:

 

```nginx
modern_browser msie      5.0;
modern_browser gecko     0.9.1;
modern_browser opera     8.0;
modern_browser safari    413;
modern_browser konqueror 3.0;

modern_browser unlisted;

ancient_browser Links Lynx netscape4;

if ($ancient_browser) {
    rewrite ^ /ancient.html;
}
```

#### Directives

            Syntax:
                

`ancient_browser string ...;`

            Default:
                

            
            —
        
                

            Context:
                

`http`, `server`, `location`

If any of the specified substrings is found in the “User-Agent”
request header field, the browser will be considered ancient.
The special string “`netscape4`” corresponds to the
regular expression “`^Mozilla/[1-4]`”.

            Syntax:
                

`ancient_browser_value string;`

            Default:
                

```nginx
ancient_browser_value 1;
```

            Context:
                

`http`, `server`, `location`

Sets a value for the `$ancient_browser` variables.

            Syntax:
                

`modern_browser browser version;`
`modern_browser unlisted;`

            Default:
                

            
            —
        
                

            Context:
                

`http`, `server`, `location`

Specifies a version starting from which a browser is considered modern.
A browser can be any one of the following: `msie`,
`gecko` (browsers based on Mozilla),
`opera`, `safari`,
or `konqueror`.

Versions can be specified in the following formats: X, X.X, X.X.X, or X.X.X.X.
The maximum values for each of the format are
4000, 4000.99, 4000.99.99, and 4000.99.99.99, respectively.

The special value `unlisted` specifies to consider
a browser as modern if it was not listed by the
`modern_browser` and [ancient_browser](#ancient_browser)
directives.
Otherwise such a browser is considered ancient.
If a request does not provide the “User-Agent” field
in the header, the browser is treated as not being listed.

            Syntax:
                

`modern_browser_value string;`

            Default:
                

```nginx
modern_browser_value 1;
```

            Context:
                

`http`, `server`, `location`

Sets a value for the `$modern_browser` variables.