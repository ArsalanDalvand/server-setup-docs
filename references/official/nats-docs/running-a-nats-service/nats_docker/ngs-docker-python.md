---
title: "Python and NGS Running in Docker"
source: "https://docs.nats.io/running-a-nats-service/nats_docker/ngs-docker-python.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_docker/ngs-docker-python"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Python and NGS Running in Docker"
---
# Python and NGS Running in Docker

Start a lightweight Docker container:

```shell
docker run --entrypoint /bin/bash -it python:3.8-slim-buster
```

Or you can also mount local creds via a volume:

```shell
docker run --entrypoint /bin/bash -v $HOME/.nkeys/creds/synadia/NGS/:/creds -it python:3.8-slim-buster
```

Install nats.py and dependencies to install nkeys:

```shell
apt-get update && apt-get install -y build-essential curl
pip install asyncio-nats-client[nkeys]
```

Get the Python examples using curl:

```shell
curl -o nats-pub.py -O -L https://raw.githubusercontent.com/nats-io/nats.py/master/examples/nats-pub/__main__.py
curl -o nats-sub.py -O -L https://raw.githubusercontent.com/nats-io/nats.py/master/examples/nats-sub/__main__.py
```

Create a subscription that lingers:

```shell
python nats-sub.py --creds /creds/NGS.creds  -s tls://connect.ngs.global:4222 hello &
```

Publish a message:

```shell
python nats-pub.py --creds /creds/NGS.creds  -s tls://connect.ngs.global:4222 hello -d world
```


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_docker/ngs-docker-python.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
