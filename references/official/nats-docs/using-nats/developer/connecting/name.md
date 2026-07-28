---
title: "Connection Name"
source: "https://docs.nats.io/using-nats/developer/connecting/name.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/name"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Connection Name"
---
# Connection Name

Connections can be assigned a name which will appear in some of the server monitoring data. This name is not required, but is **highly recommended** as a friendly connection name will help in monitoring, error reporting, debugging, and testing.

{% tabs %}
{% tab title="Go" %}

```go
nc, err := nats.Connect("demo.nats.io", nats.Name("API Name Option Example"))
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
    .connectionName("API Name Option Example") // Set Name
    .build();
Connection nc = Nats.connect(options);

// Do something with the connection

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
 const nc = await connect({
    name: "my-connection",
    servers: ["demo.nats.io:4222"],
});
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()
await nc.connect(
    servers=["nats://demo.nats.io:4222"], 
    name="API Name Option Example")

# Do something with the connection

await nc.close()
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;

await using var client = new NatsClient(name: "API Name Option Example", url: "nats://demo.nats.io:4222");

// It's optional to call ConnectAsync()
// as it will be called when needed automatically
await client.ConnectAsync();
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'

NATS.start(servers: ["nats://demo.nats.io:4222"], name: "API Name Option Example") do |nc|
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
    s = natsOptions_SetName(opts, "API Name Option Example");
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
GET https://docs.nats.io/using-nats/developer/connecting/name.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
