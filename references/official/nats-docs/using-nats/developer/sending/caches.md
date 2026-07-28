---
title: "Caches, Flush and Ping"
source: "https://docs.nats.io/using-nats/developer/sending/caches.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/sending/caches"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Caches, Flush and Ping"
---
# Caches, Flush and Ping

For performance reasons, most if not all, of the client libraries will buffer outgoing data so that bigger chunks can be written to the network at one time. This may be as simple as a byte buffer that stores a few messages before being pushed to the network.

These buffers do not hold messages forever, generally they are designed to hold messages in high throughput scenarios, while still providing good latency in low throughput situations.

It is the libraries job to make sure messages flow in a high performance manner. But there may be times when an application needs to know that a message has "hit the wire." In this case, applications can use a flush call to tell the library to move data through the system.

{% tabs %}
{% tab title="Go" %}

```go
nc, err := nats.Connect("demo.nats.io")
if err != nil {
    log.Fatal(err)
}
defer nc.Close()

// Just to not collide using the demo server with other users.
subject := nats.NewInbox()

if err := nc.Publish(subject, []byte("All is Well")); err != nil {
    log.Fatal(err)
}
// Sends a PING and wait for a PONG from the server, up to the given timeout.
// This gives guarantee that the server has processed the above message.
if err := nc.FlushTimeout(time.Second); err != nil {
    log.Fatal(err)
}
```

{% endtab %}

{% tab title="Java" %}

```java
Connection nc = Nats.connect("nats://demo.nats.io:4222");

nc.publish("updates", "All is Well".getBytes(StandardCharsets.UTF_8));
nc.flush(Duration.ofSeconds(1)); // Flush the message queue

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const start = Date.now();
nc.flush().then(() => {
  t.log("round trip completed in", Date.now() - start, "ms");
});
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()

await nc.connect(servers=["nats://demo.nats.io:4222"])

await nc.publish("updates", b'All is Well')

# Sends a PING and wait for a PONG from the server, up to the given timeout.
# This gives guarantee that the server has processed above message.
await nc.flush(timeout=1)
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;

await using var client = new NatsClient();

await client.PublishAsync("updates", "All is well");

// Sends a PING and wait for a PONG from the server.
// This gives a guarantee that the server has processed the above message
// since the underlining TCP connection sends and receives messages in order.
await client.PingAsync();
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'
require 'fiber'

NATS.start(servers:["nats://127.0.0.1:4222"]) do |nc|
  nc.subscribe("updates") do |msg|
    puts msg
  end

  nc.publish("updates", "All is Well")

  nc.flush do
    # Sends a PING and wait for a PONG from the server, up to the given timeout.
    # This gives guarantee that the server has processed above message at this point.
  end
end
```

{% endtab %}

{% tab title="C" %}

```c
natsConnection      *conn      = NULL;
natsStatus          s          = NATS_OK;

s = natsConnection_ConnectTo(&conn, NATS_DEFAULT_URL);

// Send a request and wait for up to 1 second
if (s == NATS_OK)
    s = natsConnection_PublishString(conn, "foo", "All is Well");

// Sends a PING and wait for a PONG from the server, up to the given timeout.
// This gives guarantee that the server has processed the above message.
if (s == NATS_OK)
    s = natsConnection_FlushTimeout(conn, 1000);

(...)

// Destroy objects that were created
natsConnection_Destroy(conn);
```

{% endtab %}
{% endtabs %}

## Flush and Ping/Pong

Many of the client libraries use the [PING/PONG interaction](/using-nats/developer/connecting/pingpong.md) built into the NATS protocol to ensure that flush pushed all of the buffered messages to the server. When an application calls flush, most libraries will put a PING on the outgoing queue of messages, and wait for the server to respond with a PONG before saying that the flush was successful.

Even though the client may use PING/PONG for flush, pings sent this way do not count towards [max outgoing pings](/using-nats/developer/connecting/pingpong.md).


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/developer/sending/caches.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
