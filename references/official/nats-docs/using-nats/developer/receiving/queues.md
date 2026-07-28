---
title: "Queue Subscriptions"
source: "https://docs.nats.io/using-nats/developer/receiving/queues.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/receiving/queues"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Queue Subscriptions"
---
# Queue Subscriptions

Subscribing to a [queue group](/nats-concepts/core-nats/queue.md) is only slightly different than subscribing to a subject alone. The application simply includes a queue name with the subscription. The server will load balance between all members of the queue group. In a cluster setup, every member has the same chance of receiving a particular message.

Keep in mind that queue groups in NATS are dynamic and do not require any server configuration.

![](/files/FP4yqjIeJcTbNfzpbJcd)

As an example, to subscribe to the queue `workers` with the subject `updates`:

{% tabs %}
{% tab title="Go" %}

```go
nc, err := nats.Connect("demo.nats.io")
if err != nil {
    log.Fatal(err)
}
defer nc.Close()

// Use a WaitGroup to wait for 10 messages to arrive
wg := sync.WaitGroup{}
wg.Add(10)

// Create a queue subscription on "updates" with queue name "workers"
if _, err := nc.QueueSubscribe("updates", "workers", func(m *nats.Msg) {
    wg.Done()
}); err != nil {
    log.Fatal(err)
}

// Wait for messages to come in
wg.Wait()
```

{% endtab %}

{% tab title="Java" %}

```java
Connection nc = Nats.connect("nats://demo.nats.io:4222");

// Use a latch to wait for 10 messages to arrive
CountDownLatch latch = new CountDownLatch(10);

// Create a dispatcher and inline message handler
Dispatcher d = nc.createDispatcher((msg) -> {
    String str = new String(msg.getData(), StandardCharsets.UTF_8);
    System.out.println(str);
    latch.countDown();
});

// Subscribe to the "updates" subject with a queue group named "workers"
d.subscribe("updates", "workers");

// Wait for a message to come in
latch.await(); 

// Close the connection
nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
nc.subscribe(subj, {
    queue: "workers",
    callback: (_err, _msg) => {
      t.log("worker1 got message");
    },
});

nc.subscribe(subj, {
    queue: "workers",
    callback: (_err, _msg) => {
      t.log("worker2 got message");
    },
});
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

await nc.subscribe("updates", queue="workers", cb=cb)
await nc.publish("updates", b'All is Well')

msg = await asyncio.wait_for(future, 1)
print("Msg", msg)
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;

await using var client = new NatsClient();

var count = 0;

// Subscribe to the "updates" subject with a queue group named "workers"
await foreach (var msg in client.SubscribeAsync<string>(subject: "updates", queueGroup: "workers"))
{
    Console.WriteLine($"Received {++count}: {msg.Subject}: {msg.Data}");
    
    // Break after 10 messages
    if (count == 10)
    {
        break;
    }
}

Console.WriteLine("Done");
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'
require 'fiber'

NATS.start(servers:["nats://127.0.0.1:4222"]) do |nc|
  Fiber.new do
    f = Fiber.current

    nc.subscribe("updates", queue: "worker") do |msg, reply|
      f.resume Time.now
    end

    nc.publish("updates", "A")

    # Use the response
    msg = Fiber.yield
    puts "Msg: #{msg}"
  end.resume
end
```

{% endtab %}

{% tab title="C" %}

```c
static void
onMsg(natsConnection *conn, natsSubscription *sub, natsMsg *msg, void *closure)
{
    printf("Received msg: %s - %.*s\n",
           natsMsg_GetSubject(msg),
           natsMsg_GetDataLength(msg),
           natsMsg_GetData(msg));

    // Need to destroy the message!
    natsMsg_Destroy(msg);
}


(...)

natsConnection      *conn = NULL;
natsSubscription    *sub  = NULL;
natsStatus          s;

s = natsConnection_ConnectTo(&conn, NATS_DEFAULT_URL);

// Create a queue subscription on "updates" with queue name "workers"
if (s == NATS_OK)
    s = natsConnection_QueueSubscribe(&sub, conn, "updates", "workers", onMsg, NULL);

(...)


// Destroy objects that were created
natsSubscription_Destroy(sub);
natsConnection_Destroy(conn);
```

{% endtab %}
{% endtabs %}

If you run this example with the publish examples that send to `updates`, you will see that one of the instances gets a message while the others you run won't. But the instance that receives the message will change.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/developer/receiving/queues.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
