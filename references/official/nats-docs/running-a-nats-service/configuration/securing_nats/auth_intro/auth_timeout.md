---
title: "Authentication Timeout"
source: "https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/auth_timeout.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/securing_nats/auth_intro/auth_timeout"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Authentication Timeout"
---
# Authentication Timeout

You can specify a timeout to limit how long the server will wait for a client to authenticate.

If you don't specify a value, or if you specify the value "0", then the default will be 1 second more than the `tls_timeout`.

If you do specify an invalid value, it will use a default of 1 second.

If a client doesn't authenticate to the server within the specified time, the server disconnects the server to prevent abuses.

Timeouts are specified in seconds (and can be fractional). Unlike `tls_timeout`, you cannot use "human readable" values like `10s`, you must specify a number, which will be interpreted as seconds. `10` will be 10 seconds, `3.5` will be 3 seconds and 500 milliseconds, etc.

As with TLS timeouts, long timeouts can be an opportunity for abuse. If setting the authentication timeout, it is important to note that it should be longer than the `tls timeout` option, as the authentication timeout includes the TLS upgrade time.

```
authorization: {
    timeout: 3
    users: [
        {user: a, password b},
        {user: b, password a}
    ]
}
```


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/auth_timeout.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
