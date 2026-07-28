---
title: "Pausing Between Reconnect Attempts"
source: "https://docs.nats.io/using-nats/developer/connecting/reconnect/wait.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/reconnect/wait"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Pausing Between Reconnect Attempts"
---
# Pausing Between Reconnect Attempts

It doesn’t make much sense to try to connect to the same server over and over. To prevent this sort of thrashing, and wasted reconnect attempts, especially when using TLS, libraries provide a wait setting. Generally clients make sure that between two reconnect attempts to the **same** server at least a certain amount of time has passed. The concrete implementation depends on the library used.

This setting not only prevents wasting client resources, it also alleviates a [*thundering herd*](/using-nats/developer/connecting/reconnect/random.md) situation when additional servers are not available.

{% tabs %}
{% tab title="Go" %}

```go
// Set reconnect interval to 10 seconds
nc, err := nats.Connect("demo.nats.io", nats.ReconnectWait(10*time.Second))
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
    .server("nats://demo.nats.io:4222")
    .reconnectWait(Duration.ofSeconds(10))  // Set Reconnect Wait
    .build();
Connection nc = Nats.connect(options);

// Do something with the connection

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const nc = await connect({
    reconnectTimeWait: 10 * 1000, // 10s
    servers: ["demo.nats.io"],
});
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()
await nc.connect(
   servers=["nats://demo.nats.io:4222"],
   reconnect_time_wait=10,
   )

# Do something with the connection

await nc.close()
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;
using NATS.Client.Core;

await using var client = new NatsClient(new NatsOpts
{
    Url = "nats://127.0.0.1:1222,nats://127.0.0.1:1223,nats://127.0.0.1:1224",
    
    // Set reconnect interval to between 5-10 seconds
    ReconnectWaitMin = TimeSpan.FromSeconds(5),
    ReconnectWaitMax = TimeSpan.FromSeconds(10),
});
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'

NATS.start(servers: ["nats://127.0.0.1:1222", "nats://127.0.0.1:1223", "nats://127.0.0.1:1224"], reconnect_time_wait: 10) do |nc|
   # Do something with the connection

   # Close the connection
   nc.close
end
```

{% endtab %}

{% tab title="C" %}

```c
natsConnection      *conn      = NULL;
natsOptions         *opts      = NULL;
natsStatus          s          = NATS_OK;

s = natsOptions_Create(&opts);
if (s == NATS_OK)
    // Set reconnect interval to 10 seconds (10,000 milliseconds)
    s = natsOptions_SetReconnectWait(opts, 10000);
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
GET https://docs.nats.io/using-nats/developer/connecting/reconnect/wait.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
