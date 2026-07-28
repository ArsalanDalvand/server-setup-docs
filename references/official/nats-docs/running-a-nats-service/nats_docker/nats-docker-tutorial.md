---
title: "Tutorial"
source: "https://docs.nats.io/running-a-nats-service/nats_docker/nats-docker-tutorial.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_docker/nats-docker-tutorial"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Tutorial"
---
# Tutorial

In this tutorial you run the [NATS server Docker image](https://hub.docker.com/_/nats/). The Docker image provides an instance of the NATS Server. Synadia actively maintains and supports the nats-server Docker image. The NATS image is only 6 MB in size.

**1. Set up Docker.**

See [Get Started with Docker](http://docs.docker.com/mac/started/) for guidance.

The easiest way to run Docker is to use the [Docker Toolbox](http://docs.docker.com/mac/step_one/).

**2. Run the nats-server Docker image.**

```bash
docker run -p 4222:4222 -p 8222:8222 -p 6222:6222 --name nats-server -ti nats:latest
```

**3. Verify that the NATS server is running.**

You should see the following:

```
Unable to find image 'nats:latest' locally
latest: Pulling from library/nats
2d3d00b0941f: Pull complete 
24bc6bd33ea7: Pull complete 
Digest: sha256:47b825feb34e545317c4ad122bd1a752a3172bbbc72104fc7fb5e57cf90f79e4
Status: Downloaded newer image for nats:latest
```

Followed by this, indicating that the NATS server is running:

```
[1] 2019/06/01 18:34:19.605144 [INF] Starting nats-server version 2.0.0
[1] 2019/06/01 18:34:19.605191 [INF] Starting http monitor on 0.0.0.0:8222
[1] 2019/06/01 18:34:19.605286 [INF] Listening for client connections on 0.0.0.0:4222
[1] 2019/06/01 18:34:19.605312 [INF] Server is ready
[1] 2019/06/01 18:34:19.608756 [INF] Listening for route connections on 0.0.0.0:6222
```

Notice how quickly the NATS server Docker image downloads. It is a mere 6 MB in size.

**4. Test the NATS server to verify it is running.**

An easy way to test the client connection port is through using telnet.

```bash
telnet localhost 4222
```

Expected result:

```
Trying ::1...
Connected to localhost.
Escape character is '^]'.
INFO {"server_id":"NDP7NP2P2KADDDUUBUDG6VSSWKCW4IC5BQHAYVMLVAJEGZITE5XP7O5J","version":"2.0.0","proto":1,"go":"go1.11.10","host":"0.0.0.0","port":4222,"max_payload":1048576,"client_id":13249}
```

You can also test the monitoring endpoint, viewing `http://localhost:8222` with a browser.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_docker/nats-docker-tutorial.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
