---
title: "Sending Messages"
source: "https://docs.nats.io/using-nats/developer/sending.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/sending"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Sending Messages"
---
# Sending Messages

NATS sends and receives messages using a protocol that includes a target subject, an optional reply subject and an array of bytes. Some libraries may provide helpers to convert other data formats to and from bytes, but the NATS server will treat all messages as opaque byte arrays.

All of the NATS clients are designed to make sending a message simple. For example, to send the string “All is Well” to the “updates” subject as a UTF-8 string of bytes you would do:

{% tabs %}
{% tab title="Go" %}

```go
nc, err := nats.Connect("demo.nats.io", nats.Name("API PublishBytes Example"))
if err != nil {
    log.Fatal(err)
}
defer nc.Close()

if err := nc.Publish("updates", []byte("All is Well")); err != nil {
    log.Fatal(err)
}
```

{% endtab %}

{% tab title="Java" %}

```java
Connection nc = Nats.connect("nats://demo.nats.io:4222");

nc.publish("updates", "All is Well".getBytes(StandardCharsets.UTF_8));
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
const sc = StringCodec();
nc.publish("updates", sc.encode("All is Well"));
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()

await nc.connect(servers=["nats://demo.nats.io:4222"])

await nc.publish("updates", b'All is Well')
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;

await using var client = new NatsClient(url: "demo.nats.io", name: "API Publish String Example");

// The default serializer uses UTF-8 encoding for strings
await client.PublishAsync<string>(subject: "updates", data: "All is Well");
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'

NATS.start(servers:["nats://127.0.0.1:4222"]) do |nc|
  nc.publish("updates", "All is Well")
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
GET https://docs.nats.io/using-nats/developer/sending.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
