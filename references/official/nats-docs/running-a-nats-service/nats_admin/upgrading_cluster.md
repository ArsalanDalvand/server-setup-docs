---
title: "Upgrading a Cluster"
source: "https://docs.nats.io/running-a-nats-service/nats_admin/upgrading_cluster.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_admin/upgrading_cluster"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Upgrading a Cluster"
---
# Upgrading a Cluster

Repeat this procedure for all nodes of the cluster, one at a time:

1. Stop the server by putting it into [[running-a-nats-service/nats_admin/lame_duck_mode|Lame Duck Mode]]
2. Replace the binary or Docker image with the new version.
3. Restart the server.
4. Wait until the `/healthz` endpoint returns `HTTP 200 OK` before moving on to the next cluster node.

### Downgrading

Although the NATS server goes through rigorous testing for each release, there may be a need to revert to the previous version if you observe a performance regression for your workload. The support policy for the server is the current release as well as one patch version release prior. For example, if the latest is 2.8.4, a downgrade to 2.8.3 is supported. Downgrades to earlier versions may work, but is not recommended.

Fortunately, the downgrade path is the same as the upgrade path as noted above. Swap the binary and do a rolling restart.


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_admin/upgrading_cluster.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
