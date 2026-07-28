---
title: "Monitoring the Connection"
source: "https://docs.nats.io/using-nats/developer/connecting/events.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/events"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Monitoring the Connection"
---
# Monitoring the Connection

Managing the interaction with the server is primarily the job of the client library but most of the libraries also provide some insight into what is happening under the covers.

For example, the client library may provide a mechanism to get the connection's current status:

{% tabs %}
{% tab title="Go" %}

```go
nc, err := nats.Connect("demo.nats.io", nats.Name("API Example"))
if err != nil {
    log.Fatal(err)
}
defer nc.Close()

getStatusTxt := func(nc *nats.Conn) string {
    switch nc.Status() {
    case nats.CONNECTED:
        return "Connected"
    case nats.CLOSED:
        return "Closed"
    default:
        return "Other"
    }
}
log.Printf("The connection is %v\n", getStatusTxt(nc))

nc.Close()

log.Printf("The connection is %v\n", getStatusTxt(nc))
```

{% endtab %}

{% tab title="Java" %}

```java
Connection nc = Nats.connect("nats://demo.nats.io:4222");

System.out.println("The Connection is: " + nc.getStatus()); // CONNECTED

nc.close();

System.out.println("The Connection is: " + nc.getStatus()); // CLOSED
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
  // you can find out where you connected:
t.log(`connected to a nats server version ${nc.info.version}`);

// or information about the data in/out of the client:
const stats = nc.stats();
t.log(`client sent ${stats.outMsgs} messages and received ${stats.inMsgs}`);
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()

await nc.connect(
   servers=["nats://demo.nats.io:4222"],
   )

# Do something with the connection.

print("The connection is connected?", nc.is_connected)

while True:
  if nc.is_reconnecting:
    print("Reconnecting to NATS...")
    break
  await asyncio.sleep(1)

await nc.close()

print("The connection is closed?", nc.is_closed)
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;

await using var client = new NatsClient();

Console.WriteLine($"{client.Connection.ConnectionState}"); // Closed

await client.ConnectAsync();

Console.WriteLine($"{client.Connection.ConnectionState}"); // Open
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
NATS.start(max_reconnect_attempts: 2) do |nc|
  puts "Connect is connected?: #{nc.connected?}"

  timer = EM.add_periodic_timer(1) do
    if nc.closing?
      puts "Connection closed..."
      EM.cancel_timer(timer)
      NATS.stop
    end

    if nc.reconnecting?
      puts "Reconnecting to NATS..."
      next
    end
  end
end
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
GET https://docs.nats.io/using-nats/developer/connecting/events.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
