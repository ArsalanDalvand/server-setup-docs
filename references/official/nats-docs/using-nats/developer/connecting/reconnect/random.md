---
title: "Avoiding the Thundering Herd"
source: "https://docs.nats.io/using-nats/developer/connecting/reconnect/random.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/reconnect/random"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Avoiding the Thundering Herd"
---
# Avoiding the Thundering Herd

When a server goes down, there is a possible anti-pattern called the *Thundering Herd* where all of the clients try to reconnect immediately, thus creating a denial of service attack. In order to prevent this, most NATS client libraries randomize the servers they attempt to connect to. This setting has no effect if only a single server is used, but in the case of a cluster, randomization, or shuffling, will ensure that no one server bears the brunt of the client reconnect attempts.

However, if you want to disable the randomization process for connect and reconnect, so that servers are always checked in the same order, you can do that in most libraries with a connection option:

{% tabs %}
{% tab title="Go" %}

```go
servers := []string{"nats://127.0.0.1:1222",
    "nats://127.0.0.1:1223",
    "nats://127.0.0.1:1224",
}

nc, err := nats.Connect(strings.Join(servers, ","), nats.DontRandomize())
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
    .noRandomize() // Disable randomizing servers in the bootstrap and later discovered 
    .build();
Connection nc = Nats.connect(options);

// Do something with the connection

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const nc = await connect({
    noRandomize: false,
    servers: ["127.0.0.1:4443", "demo.nats.io"],
});
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()
await nc.connect(
   servers=[
      "nats://demo.nats.io:1222",
      "nats://demo.nats.io:1223",
      "nats://demo.nats.io:1224"
      ],
   dont_randomize=True,
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
    Url = "nats://127.0.0.1:1222,nats://127.0.0.1:1223,nats://127.0.0.1:1224",
    NoRandomize = true,
});
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'

NATS.start(servers: ["nats://127.0.0.1:1222", "nats://127.0.0.1:1223", "nats://127.0.0.1:1224"], dont_randomize_servers: true) do |nc|
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
    s = natsOptions_SetNoRandomize(opts, true);
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
GET https://docs.nats.io/using-nats/developer/connecting/reconnect/random.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
