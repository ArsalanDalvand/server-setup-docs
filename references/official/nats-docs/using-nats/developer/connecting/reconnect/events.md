---
title: "Listening for Reconnect Events"
source: "https://docs.nats.io/using-nats/developer/connecting/reconnect/events.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/reconnect/events"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Listening for Reconnect Events"
---
# Listening for Reconnect Events

Because reconnect is primarily under the covers, many libraries provide an event listener you can use to be notified of reconnect events. This event can be especially important for applications sending a lot of messages.

{% tabs %}
{% tab title="Go" %}

```go
// Connection event handlers are invoked asynchronously
// and the state of the connection may have changed when
// the callback is invoked.
nc, err := nats.Connect("demo.nats.io",
    nats.DisconnectErrHandler(func(nc *nats.Conn, err error) {
        // handle disconnect error event
    }),
    nats.ReconnectHandler(func(nc *nats.Conn) {
        // handle reconnect event
    }))
if err != nil {
    log.Fatal(err)
}
defer nc.Close()

// Do something with the connection
```

{% endtab %}

{% tab title="Java" %}

```java
class MyConnectionListener implements ConnectionListener {
    public void connectionEvent(Connection conn, Events event) {
        switch (event) {
            case CONNECTED:
                // The connection has successfully completed the handshake with the nats-server.
                break;
            case CLOSED:
                // The connection is permanently closed, either by manual action or failed reconnects
                break;
            case DISCONNECTED:
                // The connection lost its connection, but may try to reconnect if configured to
                break;
            case RECONNECTED:
                // The connection was connected, lost its connection and successfully reconnected.
                break;
            case RESUBSCRIBED:
                // The connection was reconnected and the server has been notified of all subscriptions.
                break;
            case DISCOVERED_SERVERS:
                // The connection was made aware of new servers from the current server connection.
                break;
            case LAME_DUCK:
                // connected server is coming down soon, might want to prepare for it
                break;
        }
    }
}

MyConnectionListener listener = new MyConnectionListener();

Options options = new Options.Builder()
    .server("nats://demo.nats.io:4222")
    .connectionListener(listener)
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

  (async () => {
    for await (const s of nc.status()) {
      switch (s.type) {
        case Events.Reconnect:
          t.log(`client reconnected - ${s.data}`);
          break;
        default:
      }
    }
  })().then();
});
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()

async def disconnected_cb():
   print("Got disconnected!")

async def reconnected_cb():
   # See who we are connected to on reconnect.
   print("Got reconnected to {url}".format(url=nc.connected_url.netloc))

await nc.connect(
   servers=["nats://demo.nats.io:4222"],
   reconnect_time_wait=10,
   reconnected_cb=reconnected_cb,
   disconnected_cb=disconnected_cb,
   )

# Do something with the connection.
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;

await using var client = new NatsClient();

client.Connection.ConnectionDisconnected += async (sender, args) =>
{
    Console.WriteLine($"Disconnected: {args.Message}");
};

client.Connection.ConnectionOpened += async (sender, args) =>
{
    Console.WriteLine($"Connected: {args.Message}");
};

client.Connection.ReconnectFailed += async (sender, args) =>
{
    Console.WriteLine($"Reconnect Failed: {args.Message}");
};

await client.ConnectAsync();
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'

NATS.start(servers: ["nats://127.0.0.1:1222", "nats://127.0.0.1:1223", "nats://127.0.0.1:1224"]) do |nc|
   # Do something with the connection
   nc.on_reconnect do
    puts "Got reconnected to #{nc.connected_server}"
  end

  nc.on_disconnect do |reason|
    puts "Got disconnected! #{reason}"
  end
end
```

{% endtab %}

{% tab title="C" %}

```c
static void
disconnectedCB(natsConnection *conn, void *closure)
{
    // Handle disconnect error event
}

static void
reconnectedCB(natsConnection *conn, void *closure)
{
    // Handle reconnect event
}

(...)

natsConnection      *conn      = NULL;
natsOptions         *opts      = NULL;
natsStatus          s          = NATS_OK;

s = natsOptions_Create(&opts);

// Connection event handlers are invoked asynchronously
// and the state of the connection may have changed when
// the callback is invoked.
if (s == NATS_OK)
    s = natsOptions_SetDisconnectedCB(opts, disconnectedCB, NULL);
if (s == NATS_OK)
    s = natsOptions_SetReconnectedCB(opts, reconnectedCB, NULL);

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
GET https://docs.nats.io/using-nats/developer/connecting/reconnect/events.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
