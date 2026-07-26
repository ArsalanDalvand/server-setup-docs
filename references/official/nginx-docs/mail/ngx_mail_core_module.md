---
title: "Module ngx_mail_core_module"
source: "https://nginx.org/en/docs/mail/ngx_mail_core_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mail/ngx_mail_core_module"
category: "mail"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_mail_core_module"
---
## Module ngx_mail_core_module

[Example Configuration](#example)
[Directives](#directives)
     [listen](#listen)
     [mail](#mail)
     [max_errors](#max_errors)
     [protocol](#protocol)
     [resolver](#resolver)
     [resolver_timeout](#resolver_timeout)
     [server](#server)
     [server_name](#server_name)
     [timeout](#timeout)

This module is not built by default, it should be
enabled with
the `--with-mail` configuration parameter.

#### Example Configuration

 

```nginx
worker_processes auto;

error_log /var/log/nginx/error.log info;

events {
    worker_connections  1024;
}

mail {
    server_name       mail.example.com;
    auth_http         localhost:9000/cgi-bin/nginxauth.cgi;

    imap_capabilities IMAP4rev1 UIDPLUS IDLE LITERAL+ QUOTA;

    pop3_auth         plain apop cram-md5;
    pop3_capabilities LAST TOP USER PIPELINING UIDL;

    smtp_auth         login plain cram-md5;
    smtp_capabilities "SIZE 10485760" ENHANCEDSTATUSCODES 8BITMIME DSN;
    xclient           off;

    server {
        listen   25;
        protocol smtp;
    }
    server {
        listen   110;
        protocol pop3;
        proxy_pass_error_message on;
    }
    server {
        listen   143;
        protocol imap;
    }
    server {
        listen   587;
        protocol smtp;
    }
}
```

#### Directives

            Syntax:
                

`listen
address:port
    [ssl]
    [proxy_protocol]
    [backlog=number]
    [rcvbuf=size]
    [sndbuf=size]
    [bind]
    [ipv6only=on|off]
    [multipath]
    [so_keepalive=on|off|[keepidle]:[keepintvl]:[keepcnt]];`

            Default:
                

            
            —
        
                

            Context:
                

`server`

Sets the `address` and `port` for the socket
on which the server will accept requests.
It is possible to specify just the port.
The address can also be a hostname, for example:

 

```nginx
listen 127.0.0.1:110;
listen *:110;
listen 110;     # same as *:110
listen localhost:110;
```

IPv6 addresses (0.7.58) are specified in square brackets:

 

```nginx
listen [::1]:110;
listen [::]:110;
```

UNIX-domain sockets (1.3.5) are specified with the “`unix:`”
prefix:

 

```nginx
listen unix:/var/run/nginx.sock;
```

Different servers must listen on different
`address`:`port` pairs.

The `ssl` parameter allows specifying that all
connections accepted on this port should work in SSL mode.

The `proxy_protocol` parameter (1.19.8)
allows specifying that all connections accepted on this port should use the
[PROXY
protocol](http://www.haproxy.org/download/1.8/doc/proxy-protocol.txt).
Obtained information is passed to the
[[mail/ngx_mail_auth_http_module.html#proxy_protocol|authentication server]]
and can be used to
[[mail/ngx_mail_realip_module.html|change the client address]].

The `listen` directive
can have several additional parameters specific to socket-related system calls.

 

`backlog`=`number`

sets the `backlog` parameter in the
`listen()` call that limits
the maximum length for the queue of pending connections (1.9.2).
By default,
`backlog` is set to -1 on FreeBSD, DragonFly BSD, and macOS,
and to 511 on other platforms.

`rcvbuf`=`size`

sets the receive buffer size
(the `SO_RCVBUF` option) for the listening socket (1.11.13).

`sndbuf`=`size`

sets the send buffer size
(the `SO_SNDBUF` option) for the listening socket (1.11.13).

`bind`

this parameter instructs to make a separate `bind()`
call for a given address:port pair.
The fact is that if there are several `listen` directives with
the same port but different addresses, and one of the
`listen` directives listens on all addresses
for the given port (`*:``port`), nginx will
`bind()` only to `*:``port`.
It should be noted that the `getsockname()` system call will be
made in this case to determine the address that accepted the connection.
If the `backlog`,
`rcvbuf`, `sndbuf`,
`ipv6only`, `multipath`,
or `so_keepalive` parameters
are used then for a given
`address`:`port` pair
a separate `bind()` call will always be made.

`ipv6only`=`on`|`off`

this parameter determines
(via the `IPV6_V6ONLY` socket option)
whether an IPv6 socket listening on a wildcard address `[::]`
will accept only IPv6 connections or both IPv6 and IPv4 connections.
This parameter is turned on by default.
It can only be set once on start.

`multipath`

this parameter (1.29.7) configures the
[Multipath TCP](https://datatracker.ietf.org/doc/html/rfc8684)
protocol (`IPPROTO_MPTCP`) for the listening socket.
This currently works only on Linux 5.6+.

Adding or removing this parameter will also enable
the `SO_REUSEPORT` socket option, which may have its security
[implications](http://man7.org/linux/man-pages/man7/socket.7.html).

`so_keepalive`=`on`|`off`|[`keepidle`]:[`keepintvl`]:[`keepcnt`]

this parameter configures the “TCP keepalive” behavior
for the listening socket.
If this parameter is omitted then the operating system’s settings will be
in effect for the socket.
If it is set to the value “`on`”, the
`SO_KEEPALIVE` option is turned on for the socket.
If it is set to the value “`off`”, the
`SO_KEEPALIVE` option is turned off for the socket.
Some operating systems support setting of TCP keepalive parameters on
a per-socket basis using the `TCP_KEEPIDLE`,
`TCP_KEEPINTVL`, and `TCP_KEEPCNT` socket options.
On such systems
(currently, Linux, NetBSD, Dragonfly, FreeBSD, and macOS),
they can be configured
using the `keepidle`, `keepintvl`, and
`keepcnt` parameters.
One or two parameters may be omitted, in which case the system default setting
for the corresponding socket option will be in effect.
For example,

```nginx
so_keepalive=30m::10
```

will set the idle timeout (`TCP_KEEPIDLE`) to 30 minutes,
leave the probe interval (`TCP_KEEPINTVL`) at its system default,
and set the probes count (`TCP_KEEPCNT`) to 10 probes.

            Syntax:
                

`mail { ... }`

            Default:
                

            
            —
        
                

            Context:
                

`main`

Provides the configuration file context in which the mail server directives
are specified.

            Syntax:
                

`max_errors number;`

            Default:
                

```nginx
max_errors 5;
```

            Context:
                

`mail`, `server`

This directive appeared in version 1.21.0.

Sets the number of protocol errors after which the connection is closed.

            Syntax:
                

`protocol
imap |
  pop3 |
  smtp;`

            Default:
                

            
            —
        
                

            Context:
                

`server`

Sets the protocol for a proxied server.
Supported protocols are
[[mail/ngx_mail_imap_module.html|IMAP]],
[[mail/ngx_mail_pop3_module.html|POP3]], and
[[mail/ngx_mail_smtp_module.html|SMTP]].

If the directive is not set, the protocol can be detected automatically
based on the well-known port specified in the [listen](#listen)
directive:

 

`imap`: 143, 993

`pop3`: 110, 995

`smtp`: 25, 587, 465

Unnecessary protocols can be disabled using the
[[configure.html|configuration]]
parameters `--without-mail_imap_module`,
`--without-mail_pop3_module`, and
`--without-mail_smtp_module`.

            Syntax:
                

`resolver
address ...
    [valid=time]
    [ipv4=on|off]
    [ipv6=on|off]
    [status_zone=zone];`
`resolver off;`

            Default:
                

```nginx
resolver off;
```

            Context:
                

`mail`, `server`

Configures name servers used to find the client’s hostname
to pass it to the
[[mail/ngx_mail_auth_http_module.html|authentication server]],
and in the
[[mail/ngx_mail_proxy_module.html#xclient|XCLIENT]]
command when proxying SMTP.
For example:

 

```nginx
resolver 127.0.0.1 [::1]:5353;
```

The address can be specified as a domain name or IP address,
with an optional port (1.3.1, 1.2.2).
If port is not specified, the port 53 is used.
Name servers are queried in a round-robin fashion.

 
Before version 1.1.7, only a single name server could be configured.
Specifying name servers using IPv6 addresses is supported
starting from versions 1.3.1 and 1.2.2.

By default, nginx will look up both IPv4 and IPv6 addresses while resolving.
If looking up of IPv4 or IPv6 addresses is not desired,
the `ipv4=off` (1.23.1) or
the `ipv6=off` parameter can be specified.

 
Resolving of names into IPv6 addresses is supported
starting from version 1.5.8.

By default, nginx caches answers using the TTL value of a response.
An optional `valid` parameter allows overriding it:

 

```nginx
resolver 127.0.0.1 [::1]:5353 valid=30s;
```

 
Before version 1.1.9, tuning of caching time was not possible,
and nginx always cached answers for the duration of 5 minutes.

 
To prevent DNS spoofing, it is recommended
configuring DNS servers in a properly secured trusted local network.

The optional `status_zone` parameter (1.17.1)
enables
[[http/ngx_http_api_module.html#resolvers_|collection]]
of DNS server statistics of requests and responses
in the specified `zone`.
The parameter is available as part of our
[commercial subscription](https://www.f5.com/products/nginx).

The special value `off` disables resolving.

            Syntax:
                

`resolver_timeout time;`

            Default:
                

```nginx
resolver_timeout 30s;
```

            Context:
                

`mail`, `server`

Sets a timeout for DNS operations, for example:

 

```nginx
resolver_timeout 5s;
```

            Syntax:
                

`server { ... }`

            Default:
                

            
            —
        
                

            Context:
                

`mail`

Sets the configuration for a server.

            Syntax:
                

`server_name name;`

            Default:
                

```nginx
server_name hostname;
```

            Context:
                

`mail`, `server`

Sets the server name that is used:

 

in the initial POP3/SMTP server greeting;

in the salt during the SASL CRAM-MD5 authentication;

in the `EHLO` command when connecting to the SMTP backend,
if the passing of the
[[mail/ngx_mail_proxy_module.html#xclient|XCLIENT]] command
is enabled.

If the directive is not specified, the machine’s hostname is used.

            Syntax:
                

`timeout time;`

            Default:
                

```nginx
timeout 60s;
```

            Context:
                

`mail`, `server`

Sets the timeout that is used before proxying to the backend starts.