---
title: "Module ngx_stream_mqtt_filter_module"
source: "https://nginx.org/en/docs/stream/ngx_stream_mqtt_filter_module.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "stream/ngx_stream_mqtt_filter_module"
category: "stream"
retrieved_at: "2026-07-26T15:50:52+00:00"
tags:
  - nginx
  - web-server
  - mail-proxy
  - official-documentation
aliases:
  - "Module ngx_stream_mqtt_filter_module"
---
## Module ngx_stream_mqtt_filter_module

[Example Configuration](#example)
[Directives](#directives)
     [mqtt](#mqtt)
     [mqtt_buffers](#mqtt_buffers)
     [mqtt_rewrite_buffer_size](#mqtt_rewrite_buffer_size)
     [mqtt_set_connect](#mqtt_set_connect)

The `ngx_stream_mqtt_filter_module` module (1.23.4) provides
support for Message Queuing Telemetry Transport protocol (MQTT) versions
[3.1.1](https://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html)
and
[5.0](https://docs.oasis-open.org/mqtt/mqtt/v5.0/mqtt-v5.0.html).

 
This module is available as part of our
[commercial subscription](https://www.f5.com/products/nginx).

#### Example Configuration

 

```nginx
listen            127.0.0.1:18883;
proxy_pass        backend;
proxy_buffer_size 16k;

mqtt             on;
mqtt_set_connect clientid "$client";
mqtt_set_connect username "$name";
```

#### Directives

            Syntax:
                

`mqtt on | off;`

            Default:
                

```nginx
mqtt off;
```

            Context:
                

`stream`, `server`

Enables the MQTT protocol for the given virtual server.

            Syntax:
                

`mqtt_buffers number size;`

            Default:
                

```nginx
mqtt_buffers 100 1k;
```

            Context:
                

`stream`, `server`

This directive appeared in version 1.25.1.

Sets the `number` and `size` of the buffers
used for handling MQTT messages,
for a single connection.

            Syntax:
                

`mqtt_rewrite_buffer_size size;`

            Default:
                

```nginx
mqtt_rewrite_buffer_size 4k|8k;
```

            Context:
                

`server`

 
This directive is obsolete since version 1.25.1.
The [mqtt_buffers](#mqtt_buffers)
directive should be used instead.

Sets the `size` of the buffer
used for writing a modified message.
By default, the buffer size is equal to one memory page.
This is either 4K or 8K, depending on a platform.
It can be made smaller, however.

            Syntax:
                

`mqtt_set_connect field value;`

            Default:
                

            
            —
        
                

            Context:
                

`server`

Sets the message `field`
to the given `value` for CONNECT message.
The following fields are supported:
`clientid`,
`username`, and
`password`.
The value can contain text, variables, and their combination.

Several `mqtt_set_connect` directives
can be specified on the same level:

 

```nginx
mqtt_set_connect clientid "$client";
mqtt_set_connect username "$name";
```