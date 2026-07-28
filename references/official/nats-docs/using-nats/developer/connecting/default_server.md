---
title: "Connecting to the Default Server"
source: "https://docs.nats.io/using-nats/developer/connecting/default_server.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/default_server"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Connecting to the Default Server"
---
# Connecting to the Default Server

Some libraries also provide a special way to connect to a *default* url, which is generally `nats://localhost:4222`:

{% tabs %}
{% tab title="Go" %}

```go
nc, err := nats.Connect(nats.DefaultURL)
if err != nil {
    log.Fatal(err)
}
defer nc.Close()

// Do something with the connection
```

{% endtab %}

{% tab title="Java" %}

```java
Connection nc = Nats.connect();

// Do something with the connection

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const nc = await connect();
// Do something with the connection
doSomething();
// When done close it
await nc.close();
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()
await nc.connect()

# Do something with the connection

await nc.close()
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;

await using var client = new NatsClient();

// It's optional to call ConnectAsync()
// as it will be called when needed automatically
await client.ConnectAsync();
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'

NATS.start do |nc|
   # Do something with the connection

   # Close the connection
   nc.close
end
```

{% endtab %}

{% tab title="C" %}

```c
natsConnection      *conn = NULL;
natsStatus          s;

s = natsConnection_ConnectTo(&conn, NATS_DEFAULT_URL);
if (s != NATS_OK)
  // handle error

// Destroy connection, no-op if conn is NULL.
natsConnection_Destroy(conn);
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
GET https://docs.nats.io/using-nats/developer/connecting/default_server.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
