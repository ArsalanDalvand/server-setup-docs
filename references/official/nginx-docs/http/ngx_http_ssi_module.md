---
title: "Module ngx_http_ssi_module"
source: "https://nginx.org/en/docs/http/ngx_http_ssi_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_ssi_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_ssi_module"
---
## Module ngx_http_ssi_module

[Example Configuration](#example)
[Directives](#directives)
     [ssi](#ssi)
     [ssi_last_modified](#ssi_last_modified)
     [ssi_min_file_chunk](#ssi_min_file_chunk)
     [ssi_silent_errors](#ssi_silent_errors)
     [ssi_types](#ssi_types)
     [ssi_value_length](#ssi_value_length)
[SSI Commands](#commands)
[Embedded Variables](#variables)

The `ngx_http_ssi_module` module is a filter
that processes SSI (Server Side Includes) commands in responses
passing through it.
Currently, the list of supported SSI commands is incomplete.

#### Example Configuration

 

```nginx
location / {
    ssi on;
    ...
}
```

#### Directives

            Syntax:
                

`ssi on | off;`

            Default:
                

```nginx
ssi off;
```

            Context:
                

`http`, `server`, `location`, `if in location`

Enables or disables processing of SSI commands in responses.

            Syntax:
                

`ssi_last_modified on | off;`

            Default:
                

```nginx
ssi_last_modified off;
```

            Context:
                

`http`, `server`, `location`

This directive appeared in version 1.5.1.

Allows preserving the “Last-Modified” header field
from the original response during SSI processing
to facilitate response caching.

By default, the header field is removed as contents of the response
are modified during processing and may contain dynamically generated elements
or parts that are changed independently of the original response.

            Syntax:
                

`ssi_min_file_chunk size;`

            Default:
                

```nginx
ssi_min_file_chunk 1k;
```

            Context:
                

`http`, `server`, `location`

Sets the minimum `size` for parts of a response stored on disk,
starting from which it makes sense to send them using
[[http/ngx_http_core_module.html#sendfile|sendfile]].

            Syntax:
                

`ssi_silent_errors on | off;`

            Default:
                

```nginx
ssi_silent_errors off;
```

            Context:
                

`http`, `server`, `location`

If enabled, suppresses the output of the
“`[an error occurred while processing the directive]`”
string if an error occurred during SSI processing.

            Syntax:
                

`ssi_types mime-type ...;`

            Default:
                

```nginx
ssi_types text/html;
```

            Context:
                

`http`, `server`, `location`

Enables processing of SSI commands in responses with the specified MIME types
in addition to “`text/html`”.
The special value “`*`” matches any MIME type (0.8.29).

            Syntax:
                

`ssi_value_length length;`

            Default:
                

```nginx
ssi_value_length 256;
```

            Context:
                

`http`, `server`, `location`

Sets the maximum length of parameter values in SSI commands.

#### SSI Commands

SSI commands have the following generic format:

 

```nginx
<!--# command parameter1=value1 parameter2=value2 ... -->
```

The following commands are supported:

 
`block`

Defines a block that can be used as a stub
in the `include` command.
The block can contain other SSI commands.
The command has the following parameter:

`name`

block name.

Example:

```nginx
<!--# block name="one" -->
stub
<!--# endblock -->
```

`config`

Sets some parameters used during SSI processing, namely:

`errmsg`

a string that is output if an error occurs during SSI processing.
By default, the following string is output:

```nginx
[an error occurred while processing the directive]
```

`timefmt`

a format string passed to the `strftime()` function
used to output date and time.
By default, the following format is used:

```nginx
"%A, %d-%b-%Y %H:%M:%S %Z"
```

The “`%s`” format is suitable to output time in seconds.

`echo`

Outputs the value of a variable.
The command has the following parameters:

`var`

the variable name.

`encoding`

the encoding method.
Possible values include `none`, `url`, and
`entity`.
By default, `entity` is used.

`default`

a non-standard parameter that sets a string to be output
if a variable is undefined.
By default, “`(none)`” is output.
The command

```nginx
<!--# echo var="name" default="no" -->
```

replaces the following sequence of commands:

```nginx
<!--# if expr="$name" --><!--# echo var="name" --><!--#
       else -->no<!--# endif -->
```

`if`

Performs a conditional inclusion.
The following commands are supported:

```nginx
<!--# if expr="..." -->
...
<!--# elif expr="..." -->
...
<!--# else -->
...
<!--# endif -->
```

Only one level of nesting is currently supported.
The command has the following parameter:

`expr`

expression.
An expression can be:

variable existence check:

```nginx
<!--# if expr="$name" -->
```

comparison of a variable with a text:

```nginx
<!--# if expr="$name = text" -->
<!--# if expr="$name != text" -->
```

comparison of a variable with a regular expression:

```nginx
<!--# if expr="$name = /text/" -->
<!--# if expr="$name != /text/" -->
```

If a `text` contains variables,
their values are substituted.
A regular expression can contain positional and named captures
that can later be used through variables, for example:

```nginx
<!--# if expr="$name = /(.+)@(?P<domain>.+)/" -->
    <!--# echo var="1" -->
    <!--# echo var="domain" -->
<!--# endif -->
```

`include`

Includes the result of another request into a response.
The command has the following parameters:

`file`

specifies an included file, for example:

```nginx
<!--# include file="footer.html" -->
```

`virtual`

specifies an included request, for example:

```nginx
<!--# include virtual="/remote/body.php?argument=value" -->
```

Several requests specified on one page and processed by proxied or
FastCGI/uwsgi/SCGI/gRPC servers run in parallel.
If sequential processing is desired, the `wait`
parameter should be used.

`stub`

a non-standard parameter that names the block whose
content will be output if the included request results in an empty
body or if an error occurs during the request processing, for example:

```nginx
<!--# block name="one" -->&nbsp;<!--# endblock -->
<!--# include virtual="/remote/body.php?argument=value" stub="one" -->
```

The replacement block content is processed in the included request context.

`wait`

a non-standard parameter that instructs to wait for a request to fully
complete before continuing with SSI processing, for example:

```nginx
<!--# include virtual="/remote/body.php?argument=value" wait="yes" -->
```

`set`

a non-standard parameter that instructs to write a successful result
of request processing to the specified variable,
for example:

```nginx
<!--# include virtual="/remote/body.php?argument=value" set="one" -->
```

The maximum size of the response is set by the
[[http/ngx_http_core_module.html#subrequest_output_buffer_size|subrequest_output_buffer_size]]
directive (1.13.10):

```nginx
location /remote/ {
    subrequest_output_buffer_size 64k;
    ...
}
```

Prior to version 1.13.10, only the results of responses obtained using the
[[http/ngx_http_proxy_module.html|ngx_http_proxy_module]],
[[http/ngx_http_memcached_module.html|ngx_http_memcached_module]],
[[http/ngx_http_fastcgi_module.html|ngx_http_fastcgi_module]] (1.5.6),
[[http/ngx_http_uwsgi_module.html|ngx_http_uwsgi_module]] (1.5.6),
and [[http/ngx_http_scgi_module.html|ngx_http_scgi_module]] (1.5.6)
modules could be written into variables.
The maximum size of the response was set with the
[[http/ngx_http_proxy_module.html#proxy_buffer_size|proxy_buffer_size]],
[[http/ngx_http_memcached_module.html#memcached_buffer_size|memcached_buffer_size]],
[[http/ngx_http_fastcgi_module.html#fastcgi_buffer_size|fastcgi_buffer_size]],
[[http/ngx_http_uwsgi_module.html#uwsgi_buffer_size|uwsgi_buffer_size]],
and [[http/ngx_http_scgi_module.html#scgi_buffer_size|scgi_buffer_size]]
directives.

`set`

Sets a value of a variable.
The command has the following parameters:

`var`

the variable name.

`value`

the variable value.
If an assigned value contains variables,
their values are substituted.

#### Embedded Variables

The `ngx_http_ssi_module` module supports
two embedded variables:

 
`$date_local`

current time in the local time zone.
The format is set by the `config` command
with the `timefmt` parameter.

`$date_gmt`

current time in GMT.
The format is set by the `config` command
with the `timefmt` parameter.