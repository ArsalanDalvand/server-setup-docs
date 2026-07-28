---
title: "Turning Off Echo'd Messages"
source: "https://docs.nats.io/using-nats/developer/connecting/noecho.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/noecho"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Turning Off Echo'd Messages"
---
# Turning Off Echo'd Messages

By default a NATS connection will echo messages if the connection also has interest in the published subject. This means that if a publisher on a connection sends a message to a subject any subscribers on that same connection will receive the message. Clients can opt to turn off this behavior, such that regardless of interest, the message will not be delivered to subscribers on the same connection.

The NoEcho option can be useful in BUS patterns where all applications subscribe and publish to the same subject. Usually a publish represents a state change that the application already knows about, so in the case where the application publishes an update it does not need to process the update itself.

![](/files/Nhi613FSbLGTvjvuVKhC)

Keep in mind that each connection will have to turn off echo, and that it is per connection, not per application. Also, turning echo on and off can result in a major change to your applications communications protocol since messages will flow or stop flowing based on this setting and the subscribing code won't have any indication as to why.

{% tabs %}
{% tab title="Go" %}

```go
// Turn off echo
nc, err := nats.Connect("demo.nats.io", nats.Name("API NoEcho Example"), nats.NoEcho())
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
    .noEcho() // Turn off echo
    .build();
Connection nc = Nats.connect(options);

// Do something with the connection

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const nc = await connect({
    servers: ["demo.nats.io"],
    noEcho: true,
});

const sub = nc.subscribe(subj, { callback: (_err, _msg) => {} });
nc.publish(subj);
await sub.drain();
// we won't get our own messages
t.is(sub.getProcessed(), 0);
```

{% endtab %}

{% tab title="Python" %}

```python
ncA = NATS()
ncB = NATS()

await ncA.connect(no_echo=True)
await ncB.connect()

async def handler(msg):
   # Messages sent by `ncA' will not be received.
   print("[Received] ", msg)

await ncA.subscribe("greetings", cb=handler)
await ncA.flush()
await ncA.publish("greetings", b'Hello World!')
await ncB.publish("greetings", b'Hello World!')

# Do something with the connection

await asyncio.sleep(1)
await ncA.drain()
await ncB.drain()
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
    
    // Turn off echo
    Echo = false
});
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
NATS.start("nats://demo.nats.io:4222", no_echo: true) do |nc|
  # ...
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
    s = natsOptions_SetNoEcho(opts, true);
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
GET https://docs.nats.io/using-nats/developer/connecting/noecho.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
