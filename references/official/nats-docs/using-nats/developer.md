---
title: "Developing With NATS"
source: "https://docs.nats.io/using-nats/developer.md"
source_format: official-markdown
language: "en"
doc_slug: "using-nats/developer"
category: "using-nats"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Developing With NATS"
---
# Developing With NATS

Developing with NATS involves a blend of distributed application techniques, common NATS features, and library-specific syntax. Besides this guide, most libraries provide auto-generated API documentation, along with language and platform-specific examples, guides, and other resources.

| Language   | Links                                                                                                                                                                                                                                                                                 | Supported by Synadia |
| ---------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------- |
| Golang     | [nats.go](https://github.com/nats-io/nats.go), [godoc](http://godoc.org/github.com/nats-io/nats.go)                                                                                                                                                                                   | Yes                  |
| Java       | [nats.java](https://github.com/nats-io/nats.java), [javadoc](https://javadoc.io/doc/io.nats/jnats), [nats.java examples](https://github.com/nats-io/nats.java/tree/main/src/examples/java/io/nats/examples), [java-nats-examples repo](https://github.com/nats-io/java-nats-examples) | Yes                  |
| .NET       | [nats.net](https://github.com/nats-io/nats.net), [docs](http://nats-io.github.io/nats.net/), [package](https://www.nuget.org/packages/NATS.Net)                                                                                                                                       | Yes                  |
| Rust       | [nats.rs](https://github.com/nats-io/nats.rs), [rust doc](https://docs.rs/async-nats/latest/async_nats/)                                                                                                                                                                              | Yes                  |
| JavaScript | [nats.js](https://github.com/nats-io/nats.js), [jsdoc](https://nats-io.github.io/nats.js/)                                                                                                                                                                                            | Yes                  |
| Python     | [nats.py](https://github.com/nats-io/nats.py), [doc](https://nats-io.github.io/nats.py/)                                                                                                                                                                                              | Yes                  |
| C          | [nats.c](https://github.com/nats-io/nats.c), [doc](http://nats-io.github.io/nats.c)                                                                                                                                                                                                   | Yes                  |
| Ruby       | [nats-pure.rb](https://github.com/nats-io/nats-pure.rb), [yard](https://www.rubydoc.info/gems/nats)                                                                                                                                                                                   |                      |
| Elixir     | [nats.ex](https://github.com/nats-io/nats.ex), [hex doc](https://hex.pm/packages/gnat)                                                                                                                                                                                                |                      |
| Zig        | [nats.zig](https://github.com/nats-io/nats.zig)                                                                                                                                                                                                                                       |                      |
| Swift      | [nats.swift](https://github.com/nats-io/nats.swift)                                                                                                                                                                                                                                   |                      |

Not all libraries have their own documentation, depending on the language community, but be sure to check out the client libraries' README for more information.

There are many other NATS client libraries and examples contributed and maintained by the community and available on GitHub, such as:

* [Kotlin Multiplatform client](https://github.com/n-hass/nats.kt), and Java client in [Kotlin examples](https://github.com/nats-io/kotlin-nats-examples)
* [Dart](https://github.com/dgofman/nats_client), [Dart](https://github.com/chartchuo/dart-nats) and [Dart](https://github.com/c16a/nats-dart)
* [Tcl](https://github.com/Kazmirchuk/nats-tcl)
* [Crystal](https://github.com/jgaskins/nats)
* [PHP](https://github.com/basis-company/nats.php) and [PHP](https://github.com/repejota/phpnats)
* [Pascal](https://github.com/biot2/nats.pas/blob/main/nats.core.pas)
* and many [more](https://github.com/search?o=desc\&p=1\&q=nats+client\&s=updated\&type=Repositories)...


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/using-nats/developer.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
