---
title: "Connecting to a Cluster"
source: "https://docs.nats.io/using-nats/developer/connecting/cluster.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/cluster"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Connecting to a Cluster"
---
# Connecting to a Cluster

When connecting to a cluster, there are a few things to think about.

* Passing a URL for each cluster member (semi-optional)
* The connection algorithm
* The reconnect algorithm (discussed later)
* Server provided URLs

When a client library first tries to connect it will use the list of URLs provided to the connection options or function. These URLs are usually checked in random order as to not have every client connect to the same server. The first successful connection is used. Randomization can be [explicitly disabled](/using-nats/developer/connecting/reconnect/random.md).

After a client connects to the server, the server may provide a list of URLs for additional known servers. This allows a client to connect to one server and still have other servers available during reconnect.

To ensure the initial connection, your code should include a list of reasonable *front line* or *seed* servers. Those servers may know about other members of the cluster, and may tell the client about those members. But you don't have to configure the client to pass every valid member of the cluster in the connect method.

By providing the ability to pass multiple connect options, NATS can handle the possibility of a machine going down or being unavailable to a client. By adding the ability of the server to feed clients a list of known servers as part of the client-server protocol the mesh created by a cluster can grow and change organically while the clients are running.

*Note, failure behavior is library dependent, please check the documentation for your client library on information about what happens if the connect fails.*

{% tabs %}
{% tab title="Go" %}

```go
servers := []string{"nats://127.0.0.1:1222", "nats://127.0.0.1:1223", "nats://127.0.0.1:1224"}

nc, err := nats.Connect(strings.Join(servers, ","))
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
    .server("nats://127.0.0.1:1222,nats://127.0.0.1:1223,nats://127.0.0.1:1224")
    .build();
Connection nc = Nats.connect(options);

// Do something with the connection

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const nc = await connect({
    servers: [
      "nats://demo.nats.io:4222",
      "nats://localhost:4222",
    ],
});
// Do something with the connection
doSomething();
// When done close it
await nc.close();
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()
await nc.connect(servers=[
   "nats://127.0.0.1:1222",
   "nats://127.0.0.1:1223",
   "nats://127.0.0.1:1224"
   ])

# Do something with the connection

await nc.close()
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;

await using var client = new NatsClient("nats://127.0.0.1:1222,nats://127.0.0.1:1223,nats://127.0.0.1:1224");

// It's optional to call ConnectAsync()
// as it will be called when needed automatically
await client.ConnectAsync();
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'

NATS.start(servers: ["nats://127.0.0.1:1222", "nats://127.0.0.1:1223", "nats://127.0.0.1:1224"]) do |nc|
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
const char          *servers[] = {"nats://127.0.0.1:1222", "nats://127.0.0.1:1223", "nats://127.0.0.1:1224"};

s = natsOptions_Create(&opts);
if (s == NATS_OK)
    s = natsOptions_SetServers(opts, servers, 3);
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
GET https://docs.nats.io/using-nats/developer/connecting/cluster.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
