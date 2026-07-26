---
title: "Module ngx_http_stub_status_module"
source: "https://nginx.org/en/docs/http/ngx_http_stub_status_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "http/ngx_http_stub_status_module"
category: "http"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_http_stub_status_module"
---
## Module ngx_http_stub_status_module

[Example Configuration](#example)
[Directives](#directives)
     [stub_status](#stub_status)
[Data](#data)
[Embedded Variables](#variables)

The `ngx_http_stub_status_module` module provides
access to basic status information.

This module is not built by default, it should be enabled with the
`--with-http_stub_status_module`
configuration parameter.

#### Example Configuration

 

```nginx
location = /basic_status {
    stub_status;
}
```

This configuration creates a simple web page
with basic status data which may look like as follows:

 

```nginx
Active connections: 291
server accepts handled requests
 16630948 16630948 31070465
Reading: 6 Writing: 179 Waiting: 106
```

#### Directives

            Syntax:
                

`stub_status;`

            Default:
                

            
            —
        
                

            Context:
                

`server`, `location`

The basic status information will be accessible from the surrounding location.

 
In versions prior to 1.7.5,
the directive syntax required an arbitrary argument, for example,
“`stub_status on`”.

#### Data

The following status information is provided:

 
`Active connections`

The current number of active client connections
including `Waiting` connections.

`accepts`

The total number of accepted client connections.

`handled`

The total number of handled connections.
Generally, the parameter value is the same as `accepts`
unless some resource limits have been reached
(for example, the
[[ngx_core_module.html#worker_connections|worker_connections]] limit).

`requests`

The total number of client requests.

`Reading`

The current number of connections where nginx is reading the request header.

`Writing`

The current number of connections
where nginx is writing the response back to the client.

`Waiting`

The current number of idle client connections waiting for a request.

#### Embedded Variables

The `ngx_http_stub_status_module` module
supports the following embedded variables (1.3.14):

 
`$connections_active`

same as the `Active connections` value;

`$connections_reading`

same as the `Reading` value;

`$connections_writing`

same as the `Writing` value;

`$connections_waiting`

same as the `Waiting` value.