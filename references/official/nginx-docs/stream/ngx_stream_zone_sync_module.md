---
title: "Module ngx_stream_zone_sync_module"
source: "https://nginx.org/en/docs/stream/ngx_stream_zone_sync_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "stream/ngx_stream_zone_sync_module"
category: "stream"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_stream_zone_sync_module"
---
## Module ngx_stream_zone_sync_module

[Example Configuration](#example)
[Directives](#directives)
     [zone_sync](#zone_sync)
     [zone_sync_buffers](#zone_sync_buffers)
     [zone_sync_connect_retry_interval](#zone_sync_connect_retry_interval)
     [zone_sync_connect_timeout](#zone_sync_connect_timeout)
     [zone_sync_interval](#zone_sync_interval)
     [zone_sync_recv_buffer_size](#zone_sync_recv_buffer_size)
     [zone_sync_server](#zone_sync_server)
     [zone_sync_ssl](#zone_sync_ssl)
     [zone_sync_ssl_certificate](#zone_sync_ssl_certificate)
     [zone_sync_ssl_certificate_key](#zone_sync_ssl_certificate_key)
     [zone_sync_ssl_ciphers](#zone_sync_ssl_ciphers)
     [zone_sync_ssl_conf_command](#zone_sync_ssl_conf_command)
     [zone_sync_ssl_crl](#zone_sync_ssl_crl)
     [zone_sync_ssl_name](#zone_sync_ssl_name)
     [zone_sync_ssl_password_file](#zone_sync_ssl_password_file)
     [zone_sync_ssl_protocols](#zone_sync_ssl_protocols)
     [zone_sync_ssl_server_name](#zone_sync_ssl_server_name)
     [zone_sync_ssl_trusted_certificate](#zone_sync_ssl_trusted_certificate)
     [zone_sync_ssl_verify](#zone_sync_ssl_verify)
     [zone_sync_ssl_verify_depth](#zone_sync_ssl_verify_depth)
     [zone_sync_timeout](#zone_sync_timeout)
[API endpoints](#stream_zone_sync_status)
[Starting, stopping, removing a cluster node](#controlling_cluster_node)

The `ngx_stream_zone_sync_module` module (1.13.8)
provides the necessary support for synchronizing contents of
[[stream/ngx_stream_upstream_module.html#zone|shared memory zones]]
between nodes of a cluster.
To enable synchronization for a particular zone, a corresponding module
must support this feature.
Currently, it is possible to synchronize HTTP
[[http/ngx_http_upstream_module.html#sticky|sticky]]
sessions, information about
[[http/ngx_http_limit_req_module.html|excessive HTTP requests]],
and key-value pairs both in
[[http/ngx_http_keyval_module.html|http]]
and [[stream/ngx_stream_keyval_module.html|stream]].

 
This module is available as part of our
[commercial subscription](https://www.f5.com/products/nginx).

#### Example Configuration

Minimal configuration:

 

```nginx
http {
    ...

    upstream backend {
       server backend1.example.com:8080;
       server backend2.example.com:8081;

       sticky learn
              create=$upstream_cookie_examplecookie
              lookup=$cookie_examplecookie
              zone=client_sessions:1m sync;
    }

    ...
}

stream {
    ...

    server {
        zone_sync;

        listen 127.0.0.1:12345;

        # cluster of 2 nodes
        zone_sync_server a.example.com:12345;
        zone_sync_server b.example.com:12345;

    }
```

A more complex configuration with SSL enabled
and with cluster members defined by DNS:

 

```nginx
...

stream {
    ...

    resolver 127.0.0.1 valid=10s;

    server {
        zone_sync;

        # the name resolves to multiple addresses that correspond to cluster nodes
        zone_sync_server cluster.example.com:12345 resolve;

        listen 127.0.0.1:4433 ssl;

        ssl_certificate     localhost.crt;
        ssl_certificate_key localhost.key;

        zone_sync_ssl on;

        zone_sync_ssl_certificate     localhost.crt;
        zone_sync_ssl_certificate_key localhost.key;
    }
}
```

#### Directives

            Syntax:
                

`zone_sync;`

            Default:
                

            
            —
        
                

            Context:
                

`server`

Enables the synchronization of shared memory zones between cluster nodes.
Cluster nodes are defined using [zone_sync_server](#zone_sync_server) directives.

            Syntax:
                

`zone_sync_buffers number size;`

            Default:
                

```nginx
zone_sync_buffers 8 4k|8k;
```

            Context:
                

`stream`, `server`

Sets the `number` and `size` of the
per-zone buffers used for pushing zone contents.
By default, the buffer size is equal to one memory page.
This is either 4K or 8K, depending on a platform.

 
A single buffer must be large enough to hold any entry of each zone being
synchronized.

            Syntax:
                

`zone_sync_connect_retry_interval time;`

            Default:
                

```nginx
zone_sync_connect_retry_interval 1s;
```

            Context:
                

`stream`, `server`

Defines an interval between connection attempts to another cluster node.

            Syntax:
                

`zone_sync_connect_timeout time;`

            Default:
                

```nginx
zone_sync_connect_timeout 5s;
```

            Context:
                

`stream`, `server`

Defines a timeout for establishing a connection with another cluster node.

            Syntax:
                

`zone_sync_interval time;`

            Default:
                

```nginx
zone_sync_interval 1s;
```

            Context:
                

`stream`, `server`

Defines an interval for polling updates in a shared memory zone.

            Syntax:
                

`zone_sync_recv_buffer_size size;`

            Default:
                

```nginx
zone_sync_recv_buffer_size 4k|8k;
```

            Context:
                

`stream`, `server`

Sets `size` of a per-connection receive buffer used to parse
incoming stream of synchronization messages.
The buffer size must be equal or greater than one of the
[zone_sync_buffers](#zone_sync_buffers).
By default, the buffer size is equal to
[zone_sync_buffers](#zone_sync_buffers) `size`
multiplied by `number`.

            Syntax:
                

`zone_sync_server address [resolve];`

            Default:
                

            
            —
        
                

            Context:
                

`server`

Defines the `address` of a cluster node.
The address can be specified as a domain name or IP address
with a mandatory port, or as a UNIX-domain socket path
specified after the “`unix:`” prefix.
A domain name that resolves to several IP addresses defines
multiple nodes at once.

The `resolve` parameter instructs nginx to monitor
changes of the IP addresses that correspond to a domain name of the node
and automatically modify the configuration
without the need of restarting nginx.

Cluster nodes are specified either dynamically as a single
`zone_sync_server` directive with
the `resolve` parameter, or statically as a series of several
directives without the parameter.

 
Each cluster node should be specified only once.

 
All cluster nodes should use the same configuration.

In order for the `resolve` parameter to work,
the [[stream/ngx_stream_core_module.html#resolver|resolver]] directive
must be specified in the
[[stream/ngx_stream_core_module.html#stream|stream]] block.
Example:

 

```nginx
stream {
    resolver 10.0.0.1;

    server {
        zone_sync;
        zone_sync_server cluster.example.com:12345 resolve;
        ...
    }
}
```

            Syntax:
                

`zone_sync_ssl on | off;`

            Default:
                

```nginx
zone_sync_ssl off;
```

            Context:
                

`stream`, `server`

Enables the SSL/TLS protocol for connections to another cluster server.

            Syntax:
                

`zone_sync_ssl_certificate file;`

            Default:
                

            
            —
        
                

            Context:
                

`stream`, `server`

Specifies a `file` with the certificate in the PEM format
used for authentication to another cluster server.

            Syntax:
                

`zone_sync_ssl_certificate_key file;`

            Default:
                

            
            —
        
                

            Context:
                

`stream`, `server`

Specifies a `file` with the secret key in the PEM format
used for authentication to another cluster server.

            Syntax:
                

`zone_sync_ssl_ciphers ciphers;`

            Default:
                

```nginx
zone_sync_ssl_ciphers DEFAULT;
```

            Context:
                

`stream`, `server`

Specifies the enabled ciphers for connections to another cluster server.
The ciphers are specified in the format understood by the OpenSSL library.

The full list can be viewed using the
“`openssl ciphers`” command.

            Syntax:
                

`zone_sync_ssl_conf_command name value;`

            Default:
                

            
            —
        
                

            Context:
                

`stream`, `server`

This directive appeared in version 1.19.4.

Sets arbitrary OpenSSL configuration
[commands](https://www.openssl.org/docs/man1.1.1/man3/SSL_CONF_cmd.html)
when establishing a connection with another cluster server.

 
The directive is supported when using OpenSSL 1.0.2 or higher.

Several `zone_sync_ssl_conf_command` directives
can be specified on the same level.
These directives are inherited from the previous configuration level
if and only if there are
no `zone_sync_ssl_conf_command` directives
defined on the current level.

 
Note that configuring OpenSSL directly
might result in unexpected behavior.

            Syntax:
                

`zone_sync_ssl_crl file;`

            Default:
                

            
            —
        
                

            Context:
                

`stream`, `server`

Specifies a `file` with revoked certificates (CRL)
in the PEM format used to [verify](#zone_sync_ssl_verify)
the certificate of another cluster server.
When using intermediate certificates, their CRLs should be
specified in the same file.

            Syntax:
                

`zone_sync_ssl_name name;`

            Default:
                

```nginx
zone_sync_ssl_name host from zone_sync_server;
```

            Context:
                

`stream`, `server`

This directive appeared in version 1.15.7.

Allows overriding the server name used to
[verify](#zone_sync_ssl_verify)
the certificate of a cluster server and to be
[passed through SNI](#zone_sync_ssl_server_name)
when establishing a connection with the cluster server.

By default, the host part of the [zone_sync_server](#zone_sync_server) address is used,
or resolved IP address if the [resolve](#resolve) parameter is specified.

            Syntax:
                

`zone_sync_ssl_password_file file;`

            Default:
                

            
            —
        
                

            Context:
                

`stream`, `server`

Specifies a `file` with passphrases for
[secret keys](#zone_sync_ssl_certificate_key)
where each passphrase is specified on a separate line.
Passphrases are tried in turn when loading the key.

            Syntax:
                

`zone_sync_ssl_protocols 
    [SSLv2]
    [SSLv3]
    [TLSv1]
    [TLSv1.1]
    [TLSv1.2]
    [TLSv1.3];`

            Default:
                

```nginx
zone_sync_ssl_protocols TLSv1.2 TLSv1.3;
```

            Context:
                

`stream`, `server`

Enables the specified protocols for connections to another cluster server.

            Syntax:
                

`zone_sync_ssl_server_name on | off;`

            Default:
                

```nginx
zone_sync_ssl_server_name off;
```

            Context:
                

`stream`, `server`

This directive appeared in version 1.15.7.

Enables or disables passing of the server name through
[TLS
Server Name Indication extension](http://en.wikipedia.org/wiki/Server_Name_Indication) (SNI, RFC 6066)
when establishing a connection with another cluster server.

            Syntax:
                

`zone_sync_ssl_trusted_certificate file;`

            Default:
                

            
            —
        
                

            Context:
                

`stream`, `server`

Specifies a `file` with trusted CA certificates in the PEM format
used to [verify](#zone_sync_ssl_verify)
the certificate of another cluster server.

            Syntax:
                

`zone_sync_ssl_verify on | off;`

            Default:
                

```nginx
zone_sync_ssl_verify off;
```

            Context:
                

`stream`, `server`

Enables or disables verification of another cluster server certificate.

            Syntax:
                

`zone_sync_ssl_verify_depth number;`

            Default:
                

```nginx
zone_sync_ssl_verify_depth 1;
```

            Context:
                

`stream`, `server`

Sets the verification depth in another cluster server certificates chain.

            Syntax:
                

`zone_sync_timeout timeout;`

            Default:
                

```nginx
zone_sync_timeout 5s;
```

            Context:
                

`stream`, `server`

Sets the `timeout` between two successive
read or write operations on connection to another cluster node.
If no data is transmitted within this time, the connection is closed.

#### API endpoints

The synchronization status of a node is available via the
[[http/ngx_http_api_module.html#stream_zone_sync_|/stream/zone_sync/]]
endpoint of the API which returns the
[[http/ngx_http_api_module.html#def_nginx_stream_zone_sync|following]]
metrics.

#### Starting, stopping, removing a cluster node

To start a new node, update a DNS record of a cluster hostname
with the IP address of the new node and start an instance.
The new node will discover other nodes from DNS or static configuration
and will start sending updates to them.
Other nodes will eventually discover the new node using DNS and
start pushing updates to it.
In case of static configuration,
other nodes need to be reloaded in order to send updates to the new node.

To stop a node, send the `QUIT` signal to the instance.
The node will finish zone synchronization
and gracefully close open connections.

To remove a node, update a DNS record of a cluster hostname
and remove the IP address of the node.
All other nodes will eventually discover that the node is removed,
close connections to the node, and will no longer try to connect to it.
After the node is removed, it can be stopped as described above.
In case of static configuration, other nodes need to be reloaded
in order to stop sending updates to the removed node.