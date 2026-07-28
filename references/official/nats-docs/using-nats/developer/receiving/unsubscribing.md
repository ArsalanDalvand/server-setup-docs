---
title: "Unsubscribing"
source: "https://docs.nats.io/using-nats/developer/receiving/unsubscribing.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/receiving/unsubscribing"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Unsubscribing"
---
# Unsubscribing

The client libraries provide a means to unsubscribe a previous subscription request.

This process requires an interaction with the server, so for an asynchronous subscription there may be a small window of time where a message comes through as the unsubscribe is processed by the library. Ignoring that slight edge case, the client library will clean up any outstanding messages and tell the server that the subscription is no longer used.

{% tabs %}
{% tab title="Go" %}

```go
nc, err := nats.Connect("demo.nats.io")
if err != nil {
    log.Fatal(err)
}
defer nc.Close()

// Sync Subscription
sub, err := nc.SubscribeSync("updates")
if err != nil {
    log.Fatal(err)
}
if err := sub.Unsubscribe(); err != nil {
    log.Fatal(err)
}

// Async Subscription
sub, err = nc.Subscribe("updates", func(_ *nats.Msg) {})
if err != nil {
    log.Fatal(err)
}
if err := sub.Unsubscribe(); err != nil {
    log.Fatal(err)
}
```

{% endtab %}

{% tab title="Java" %}

```java
Connection nc = Nats.connect("nats://demo.nats.io:4222");
Dispatcher d = nc.createDispatcher((msg) -> {
    String str = new String(msg.getData(), StandardCharsets.UTF_8);
    System.out.println(str);
});

// Sync Subscription have an unsubscribe API
Subscription sub = nc.subscribe("updates");
sub.unsubscribe();

// Async Subscriptions on the dispatcher must unsubscribe via the dispatcher,
// not the subscription
d.subscribe("updates");
d.unsubscribe("updates");

// Close the connection
nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const sc = StringCodec();
// set up a subscription to process a request
const sub = nc.subscribe(createInbox(), (_err, m) => {
  m.respond(sc.encode(new Date().toLocaleTimeString()));
});
// without arguments the subscription will cancel when the server receives it
// you can also specify how many messages are expected by the subscription
sub.unsubscribe();
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()

await nc.connect(servers=["nats://demo.nats.io:4222"])

future = asyncio.Future()

async def cb(msg):
  nonlocal future
  future.set_result(msg)

sub = await nc.subscribe("updates", cb=cb)
await nc.publish("updates", b'All is Well')

# Remove interest in subject
await sub.unsubscribe()

# Won't be received...
await nc.publish("updates", b'...')
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;

await using var client = new NatsClient();

// Cancel the subscription after 10 seconds
using var cts = new CancellationTokenSource(TimeSpan.FromSeconds(10));

// Subscribe to the "updates" subject
// We unsubscribe when we receive the message "exit"
// or when the cancellation token is triggered.
await foreach (var msg in client.SubscribeAsync<string>("updates").WithCancellation(cts.Token))
{
    Console.WriteLine($"Received: {msg.Data}");
    
    if (msg.Data == "exit")
    {
        // When we exit the loop, we unsubscribe from the subject
        break;
    }
}

Console.WriteLine("Unsubscribed from updates");
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'
require 'fiber'

NATS.start(servers:["nats://127.0.0.1:4222"]) do |nc|
  Fiber.new do
    f = Fiber.current

    sid = nc.subscribe("time") do |msg, reply|
      f.resume Time.now
    end

    nc.publish("time", 'What is the time?', NATS.create_inbox)

    # Use the response
    msg = Fiber.yield
    puts "Reply: #{msg}"

    nc.unsubscribe(sid)

    # Won't be received
    nc.publish("time", 'What is the time?', NATS.create_inbox)

  end.resume
end
```

{% endtab %}

{% tab title="C" %}

```c
natsConnection      *conn      = NULL;
natsSubscription    *sub       = NULL;
natsStatus          s          = NATS_OK;

s = natsConnection_ConnectTo(&conn, NATS_DEFAULT_URL);

// Subscribe
if (s == NATS_OK)
    s = natsConnection_SubscribeSync(&sub, conn, "updates");

// Unsubscribe
if (s == NATS_OK)
    s = natsSubscription_Unsubscribe(sub);

(...)

// Destroy objects that were created
natsSubscription_Destroy(sub);
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
GET https://docs.nats.io/using-nats/developer/receiving/unsubscribing.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
