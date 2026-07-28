---
title: "Authenticating with a User and Password"
source: "https://docs.nats.io/using-nats/developer/connecting/userpass.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer/connecting/userpass"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Authenticating with a User and Password"
---
# Authenticating with a User and Password

For this example, start the server using:

```bash
nats-server --user myname --pass password
```

You can encrypt passwords to pass to `nats-server` using the simple [NATS CLI tool:](/using-nats/nats-tools/nats_cli.md)

```bash
nats server passwd
```

```
? Enter password [? for help] **********************
? Reenter password [? for help] **********************

$2a$11$qbtrnb0mSG2eV55xoyPqHOZx/lLBlryHRhU3LK2oOPFRwGF/5rtGK
```

and use the hashed password in the server config. The client still uses the plain text version.

The code uses localhost:4222 so that you can start the server on your machine to try them out.

## Connecting with a User/Password

When logging in with a password `nats-server` will take either a plain text password or an encrypted password.

{% tabs %}
{% tab title="Go" %}

```go
// Set a user and plain text password
nc, err := nats.Connect("127.0.0.1", nats.UserInfo("myname", "password"))
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
    .server("nats://localhost:4222")
    .userInfo("myname","password") // Set a user and plain text password
    .build();
Connection nc = Nats.connect(options);

// Do something with the connection

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
 const nc = await connect({
      port: ns.port,
      user: "byname",
      pass: "password",
});
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()

await nc.connect(servers=["nats://myname:password@demo.nats.io:4222"])

# Do something with the connection.
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;
using NATS.Client.Core;

await using var client = new NatsClient(new NatsOpts
{
    Url = "nats://localhost:4222",
    AuthOpts = new NatsAuthOpts
    {
        Username = "myname",
        Password = "password",
    }
});
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'

NATS.start(servers:["nats://myname:password@127.0.0.1:4222"], name: "my-connection") do |nc|
   nc.on_error do |e|
    puts "Error: #{e}"
  end

   nc.on_reconnect do
    puts "Got reconnected to #{nc.connected_server}"
  end

  nc.on_disconnect do |reason|
    puts "Got disconnected! #{reason}"
  end

  nc.close
end
```

{% endtab %}

{% tab title="C" %}

```c
natsConnection      *conn      = NULL;
natsOptions         *opts      = NULL;
natsStatus          s          = NATS_OK;

s = natsOptions_Create(&opts);
if (s == NATS_OK)
    s = natsOptions_SetUserInfo(opts, "myname", "password");
if (s == NATS_OK)
    s = natsConnection_Connect(&conn, opts);

(...)

// Destroy objects that were created
natsConnection_Destroy(conn);
natsOptions_Destroy(opts);
```

{% endtab %}
{% endtabs %}

## Connecting with a User/Password in the URL

Most clients make it easy to pass the user name and password by accepting them in the URL for the server. This standard format is:

> nats\://*user*:*password*@server:port

Using this format, you can connect to a server using authentication as easily as you connected with a URL:

{% tabs %}
{% tab title="Go" %}

```go
// Set a user and plain text password
nc, err := nats.Connect("myname:password@127.0.0.1")
if err != nil {
    log.Fatal(err)
}
defer nc.Close()

// Do something with the connection
```

{% endtab %}

{% tab title="Java" %}

```java
Connection nc = Nats.connect("nats://myname:password@localhost:4222");

// Do something with the connection

nc.close();
```

{% endtab %}

{% tab title="JavaScript" %}

```javascript
// JavaScript clients don't support username/password in urls use `user` and `pass` options.
```

{% endtab %}

{% tab title="Python" %}

```python
nc = NATS()

await nc.connect(servers=["nats://myname:password@demo.nats.io:4222"])

# Do something with the connection.
```

{% endtab %}

{% tab title="C#" %}

```csharp
// dotnet add package NATS.Net
using NATS.Net;
using NATS.Client.Core;

await using var nc = new NatsClient(new NatsOpts
{
    // .NET client doesn't support username/password in URLs
    // use `Username` and `Password` options.
    Url = "nats://demo.nats.io:4222",
    AuthOpts = new NatsAuthOpts
    {
        Username = "myname",
        Password = "password",
    }
});
```

{% endtab %}

{% tab title="Ruby" %}

```ruby
require 'nats/client'

NATS.start(servers:["nats://myname:password@127.0.0.1:4222"], name: "my-connection") do |nc|
   nc.on_error do |e|
    puts "Error: #{e}"
  end

   nc.on_reconnect do
    puts "Got reconnected to #{nc.connected_server}"
  end

  nc.on_disconnect do |reason|
    puts "Got disconnected! #{reason}"
  end

  nc.close
end
```

{% endtab %}

{% tab title="C" %}

```c
natsConnection      *conn      = NULL;
natsOptions         *opts      = NULL;
natsStatus          s          = NATS_OK;

s = natsOptions_Create(&opts);
if (s == NATS_OK)
    s = natsOptions_SetURL(opts, "nats://myname:password@127.0.0.1:4222");
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
GET https://docs.nats.io/using-nats/developer/connecting/userpass.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
