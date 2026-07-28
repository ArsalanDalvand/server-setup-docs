---
title: "Setting a Connect Timeout"
source: "https://docs.nats.io/using-nats/developer/connecting/connect_timeout.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/connect_timeout"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Setting a Connect Timeout"
---
# Setting a Connect Timeout

Each library has its own, language preferred way, to pass connection options. One of the most common options is a connect timeout. It limits how long it can take to establish a connection to a server. Should multiple URLs be provided, this timeout applies to each cluster member individually. To set the maximum time to connect to a server to 10 seconds:

{% tabs %}
{% tab title="Go" %}

```go
nc, err := nats.Connect("demo.nats.io", nats.Name("API Options Example"), nats.Timeout(10*time.Second))
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
    .connectionTimeout(Duration.ofSeconds(10)) // Set timeout
    .build();
Connection nc = Nats.connect(options);

// Do something with the connection

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const nc = await connect({
    timeout: 10 * 1000, // 10s
    servers: ["demo.nats.io"],
});
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()
await nc.connect(
  servers=["nats://demo.nats.io:4222"],
  connect_timeout=10)

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
    ConnectTimeout = TimeSpan.FromSeconds(10)
});

// You don't have to call ConnectAsync() explicitly,
// first operation will make the connection otherwise.
await client.ConnectAsync();
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
# There is currently no connect timeout as part of the Ruby NATS client API, but you can use a timer to mimic it.
require 'nats/client'

timer = EM.add_timer(10) do
  NATS.connect do |nc|
    # Do something with the connection

    # Close the connection
    nc.close
  end
end
EM.cancel_timer(timer)
```

{% endtab %}

{% tab title="C" %}

```c
nnatsConnection      *conn    = NULL;
 natsOptions         *opts    = NULL;
 natsStatus          s        = NATS_OK;

 s = natsOptions_Create(&opts);
 if (s == NATS_OK)
     // Set the timeout to 10 seconds (10,000 milliseconds)
     s = natsOptions_SetTimeout(opts, 10000);
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
GET https://docs.nats.io/using-nats/developer/connecting/connect_timeout.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
