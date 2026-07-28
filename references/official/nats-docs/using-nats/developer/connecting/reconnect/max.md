---
title: "Set the Number of Reconnect Attempts"
source: "https://docs.nats.io/using-nats/developer/connecting/reconnect/max.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/reconnect/max"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Set the Number of Reconnect Attempts"
---
# Set the Number of Reconnect Attempts

Applications can set the maximum reconnect attempts per server. This includes the server provided to the client's connect call, as well as the server the client discovered through another server. Once reconnect to a server fails the specified amount of times in a row, it will be removed from the connect list. After a successful reconnect to a server, the client will reset that server's failed reconnect attempt count. If a server was removed from the connect list, it can be rediscovered on connect. This effectively resets the connect attempt count as well. If the client runs out of servers to reconnect, it will close the connection and [raise an error](/using-nats/developer/connecting/reconnect/events.md).

{% tabs %}
{% tab title="Go" %}

```go
// Set max reconnects attempts
nc, err := nats.Connect("demo.nats.io", nats.MaxReconnects(10))
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
    .maxReconnects(10) // Set max reconnect attempts
    .build();
Connection nc = Nats.connect(options);

// Do something with the connection

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const nc = await connect({
    maxReconnectAttempts: 10,
    servers: ["demo.nats.io"],
});
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()
await nc.connect(
   servers=["nats://demo.nats.io:4222"],
   max_reconnect_attempts=10,
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
    Url = "nats://demo.nats.io:4222",
    
    // Set the maximum number of reconnect attempts
    MaxReconnectRetry = 10,
});
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'

NATS.start(servers: ["nats://127.0.0.1:1222", "nats://127.0.0.1:1223", "nats://127.0.0.1:1224"], max_reconnect_attempts: 10) do |nc|
   # Do something with the connection

   # Close the connection
   nc.close
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
    s = natsOptions_SetMaxReconnect(opts, 10);
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
GET https://docs.nats.io/using-nats/developer/connecting/reconnect/max.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
