---
title: "Module ngx_http_upstream_conf_module"
source: "https://nginx.org/en/docs/http/ngx_http_upstream_conf_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_upstream_conf_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_upstream_conf_module"
---
## Module ngx_http_upstream_conf_module

[Example Configuration](#example)
[Directives](#directives)
     [upstream_conf](#upstream_conf)

The `ngx_http_upstream_conf_module` module
allows configuring upstream server groups on-the-fly
via a simple HTTP interface without the need of restarting nginx.
The
[[http/ngx_http_upstream_module.html#zone|http]]
or
[[stream/ngx_stream_upstream_module.html#zone|stream]]
server group must reside in the shared memory.

 
This module was available as part of our
[commercial subscription](https://www.f5.com/products/nginx)
until 1.13.10.
It was superseded by the
[[http/ngx_http_api_module.html|ngx_http_api_module]] module
in 1.13.3.

#### Example Configuration

 

```nginx
upstream backend {
    zone upstream_backend 64k;

    ...
}

server {
    location /upstream_conf {
        upstream_conf;
        allow 127.0.0.1;
        deny all;
    }
}
```

#### Directives

            Syntax:
                

`upstream_conf;`

            Default:
                

            
            —
        
                

            Context:
                

`location`

Turns on the HTTP interface of upstream configuration in the surrounding
location.
Access to this location should be
[[http/ngx_http_core_module.html#satisfy|limited]].

Configuration commands can be used to:

 
view the group configuration;
view, modify, or remove a server;
add a new server.

 
Since addresses in a group are not required to be unique, specific
servers in a group are referenced by their IDs.
IDs are assigned automatically and shown when adding a new server
or viewing the group configuration.

A configuration command consists of parameters passed as request arguments,
for example:

 

```nginx
http://127.0.0.1/upstream_conf?upstream=backend
```

The following parameters are supported:

 

`stream=`

Selects a
[[stream/ngx_stream_upstream_module.html|stream]]
upstream server group.
Without this parameter, selects an
[[http/ngx_http_upstream_module.html|http]]
upstream server group.

`upstream=``name`

Selects a group to work with.
This parameter is mandatory.

`id=``number`

Selects a server for viewing, modifying, or removing.

`remove=`

Removes a server from the group.

`add=`

Adds a new server to the group.

`backup=`

Required to add a backup server.

Before version 1.7.2, `backup=`
was also required to view, modify, or remove existing backup servers.

`server=``address`

Same as the “`address`” parameter
of the
[[http/ngx_http_upstream_module.html#server|http]]
or
[[stream/ngx_stream_upstream_module.html#server|stream]]
upstream server.

When adding a server, it is possible to specify it as a domain name.
In this case, changes of the IP addresses that correspond to a domain name
will be monitored and automatically applied to the upstream
configuration without the need of restarting nginx (1.7.2).
This requires the “`resolver`” directive in the
[[http/ngx_http_core_module.html#resolver|http]]
or
[[stream/ngx_stream_core_module.html#resolver|stream]]
block.
See also the “`resolve`” parameter
of the
[[http/ngx_http_upstream_module.html#resolve|http]]
or
[[stream/ngx_stream_upstream_module.html#resolve|stream]]
upstream server.

`service=``name`

Same as the “`service`” parameter
of the
[[http/ngx_http_upstream_module.html#service|http]]
or
[[stream/ngx_stream_upstream_module.html#service|stream]]
upstream server (1.9.13).

`weight=``number`

Same as the “`weight`” parameter
of the
[[http/ngx_http_upstream_module.html#weight|http]]
or
[[stream/ngx_stream_upstream_module.html#weight|stream]]
upstream server.

`max_conns=``number`

Same as the “`max_conns`” parameter
of the
[[http/ngx_http_upstream_module.html#max_conns|http]]
or
[[stream/ngx_stream_upstream_module.html#max_conns|stream]]
upstream server.

`max_fails=``number`

Same as the “`max_fails`” parameter
of the
[[http/ngx_http_upstream_module.html#max_fails|http]]
or
[[stream/ngx_stream_upstream_module.html#max_fails|stream]]
upstream server.

`fail_timeout=``time`

Same as the “`fail_timeout`” parameter
of the
[[http/ngx_http_upstream_module.html#fail_timeout|http]]
or
[[stream/ngx_stream_upstream_module.html#fail_timeout|stream]]
upstream server.

`slow_start=``time`

Same as the “`slow_start`” parameter
of the
[[http/ngx_http_upstream_module.html#slow_start|http]]
or
[[stream/ngx_stream_upstream_module.html#slow_start|stream]]
upstream server.

`down=`

Same as the “`down`” parameter
of the
[[http/ngx_http_upstream_module.html#down|http]]
or
[[stream/ngx_stream_upstream_module.html#down|stream]]
upstream server.

`drain=`

Puts the
[[http/ngx_http_upstream_module.html|http]]
upstream server into the “draining” mode (1.7.5).
In this mode, only requests
[[http/ngx_http_upstream_module.html#sticky|bound]] to the server
will be proxied to it.

`up=`

The opposite of the “`down`” parameter
of the
[[http/ngx_http_upstream_module.html#down|http]]
or
[[stream/ngx_stream_upstream_module.html#down|stream]]
upstream server.

`route=``string`

Same as the “`route`” parameter of the
[[http/ngx_http_upstream_module.html#route|http]]
upstream server.

The first three parameters select an object.
This can be either the whole http or stream upstream server group,
or a specific server.
Without other parameters, the configuration of the selected
group or server is shown.

For example, to view the configuration of the whole group, send:

 

```nginx
http://127.0.0.1/upstream_conf?upstream=backend
```

To view the configuration of a specific server, also specify its ID:

 

```nginx
http://127.0.0.1/upstream_conf?upstream=backend&id=42
```

To add a new server,
specify its address in the “`server=`” parameter.
Without other parameters specified, a server will be added with other
parameters set to their default values (see the
[[http/ngx_http_upstream_module.html#server|http]]
or
[[stream/ngx_stream_upstream_module.html#server|stream]]
“`server`” directive).

For example, to add a new primary server, send:

 

```nginx
http://127.0.0.1/upstream_conf?add=&upstream=backend&server=127.0.0.1:8080
```

To add a new backup server, send:

 

```nginx
http://127.0.0.1/upstream_conf?add=&upstream=backend&backup=&server=127.0.0.1:8080
```

To add a new primary server,
set its parameters to non-default values
and mark it as “`down`”, send:

 

```nginx
http://127.0.0.1/upstream_conf?add=&upstream=backend&server=127.0.0.1:8080&weight=2&down=
```

To remove a server, specify its ID:

 

```nginx
http://127.0.0.1/upstream_conf?remove=&upstream=backend&id=42
```

To mark an existing server as “`down`”, send:

 

```nginx
http://127.0.0.1/upstream_conf?upstream=backend&id=42&down=
```

To modify the address of an existing server, send:

 

```nginx
http://127.0.0.1/upstream_conf?upstream=backend&id=42&server=192.0.2.3:8123
```

To modify other parameters of an existing server, send:

 

```nginx
http://127.0.0.1/upstream_conf?upstream=backend&id=42&max_fails=3&weight=4
```

The above examples are for an
[[http/ngx_http_upstream_module.html|http]]
upstream server group.
Similar examples for a
[[stream/ngx_stream_upstream_module.html|stream]]
upstream server group require the “`stream=`” parameter.