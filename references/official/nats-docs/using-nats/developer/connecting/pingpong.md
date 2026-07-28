---
title: "Ping/Pong Protocol"
source: "https://docs.nats.io/using-nats/developer/connecting/pingpong.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/pingpong"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Ping/Pong Protocol"
---
# Ping/Pong Protocol

NATS client applications use a PING/PONG protocol to check that there is a working connection to the NATS service. Periodically the client will send PING messages to the server, which responds with a PONG. This period is configured by specifying a ping interval on the client connection settings.

![](/files/s7qTcrU8mhTgxLoELl6S)

The connection will be closed as stale when the client reaches a number of pings which recieved no pong in response, which is configured by specifying the maximum pings outstanding on the client connection settings.

The ping interval and the maximum pings outstanding work together to specify how quickly the client connection will be notified of a problem. This will also help when there is a remote network partition where the operating system does not detect a socket error. Upon connection close, the client will attempt to reconnect. When it knows about other servers, these will be tried next.

In the presence of traffic, such as messages or client side pings, the server will not initiate the PING/PONG interaction.

On connections with significant traffic, the client will often figure out there is a problem between PINGS, and as a result the default ping interval is typically on the order of minutes. To close an unresponsive connection after 100s, set the ping interval to 20s and the maximum pings outstanding to 5:

{% tabs %}
{% tab title="Go" %}

```go
// Set Ping Interval to 20 seconds and Max Pings Outstanding to 5
nc, err := nats.Connect("demo.nats.io", nats.Name("API Ping Example"), nats.PingInterval(20*time.Second), nats.MaxPingsOutstanding(5))
if err != nil {
    log.Fatal(err)
}
defer nc.Close()

// Do something with the connection
```

{% endtab %}

{% tab title="Java" %}

```java
Options options = new Options.Builder()
    .server("nats://demo.nats.io")
    .pingInterval(Duration.ofSeconds(20)) // Set Ping Interval
    .maxPingsOut(5) // Set max pings in flight
    .build();

// Connection is AutoCloseable
try (Connection nc = Nats.connect(options)) {
    // Do something with the connection
}
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
// Set Ping Interval to 20 seconds and Max Pings Outstanding to 5
const nc = await connect({
    pingInterval: 20 * 1000,
    maxPingOut: 5,
    servers: ["demo.nats.io:4222"],
});
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()

await nc.connect(
   servers=["nats://demo.nats.io:4222"],
   # Set Ping Interval to 20 seconds and Max Pings Outstanding to 5
   ping_interval=20,
   max_outstanding_pings=5,
   )

# Do something with the connection.
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;
using NATS.Client.Core;

await using var client = new NatsClient(new NatsOpts
{
    Url = "nats://demo.nats.io:4222",
    
    // Set Ping Interval to 20 seconds and Max Pings Outstanding to 5
    PingInterval = TimeSpan.FromSeconds(20),
    MaxPingOut = 5,
});
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'
# Set Ping Interval to 20 seconds and Max Pings Outstanding to 5
NATS.start(ping_interval: 20, max_outstanding_pings: 5) do |nc|
   nc.on_reconnect do
    puts "Got reconnected to #{nc.connected_server}"
  end

  nc.on_disconnect do |reason|
    puts "Got disconnected! #{reason}"
  end

  # Do something with the connection
end
```

{% endtab %}

{% tab title="C" %}

```c
natsConnection      *conn    = NULL;
natsOptions         *opts    = NULL;
natsStatus          s        = NATS_OK;

s = natsOptions_Create(&opts);
if (s == NATS_OK)
    // Set Ping interval to 20 seconds (20,000 milliseconds)
    s = natsOptions_SetPingInterval(opts, 20000);
if (s == NATS_OK)
    // Set the limit to 5
    s = natsOptions_SetMaxPingsOut(opts, 5);
if (s == NATS_OK)
    s = natsConnection_Connect(&conn, opts);

(...)

// Destroy objects that were created
natsConnection_Destroy(conn);
natsOptions_Destroy(opts);
```

{% endtab %}
{% endtabs %}


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/developer/connecting/pingpong.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
