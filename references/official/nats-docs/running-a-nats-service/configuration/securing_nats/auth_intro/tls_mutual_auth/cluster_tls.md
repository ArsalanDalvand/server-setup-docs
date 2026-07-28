---
title: "TLS Authentication in clusters"
source: "https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/tls_mutual_auth/cluster_tls.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/securing_nats/auth_intro/tls_mutual_auth/cluster_tls"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "TLS Authentication in clusters"
---
# TLS Authentication in clusters

When setting up clusters, all servers in the cluster, if using TLS, will both verify the connecting endpoints and the server responses. So certificates are checked in [both directions](/running-a-nats-service/configuration/securing_nats/tls.md#wrong-key-usage). Certificates can be configured only for the server's cluster identity, keeping client and server certificates separate from cluster formation.

TLS Mutual Authentication *is the only way* of securing routes.

```
cluster {
  listen: 127.0.0.1:4244

  tls {
    # Route cert
    cert_file: "./configs/certs/srva-cert.pem"
    # Private key
    key_file:  "./configs/certs/srva-key.pem"
    # Optional certificate authority verifying connected routes
    # Required when we have self-signed CA, etc.
    ca_file:   "./configs/certs/ca.pem"
  }
  # Routes are actively solicited and connected to from this server.
  # Other servers can connect to us if they supply the correct credentials
  # in their routes definitions from above.
  routes = [
    nats://127.0.0.1:4246
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
GET https://docs.nats.io/running-a-nats-service/configuration/securing_nats/auth_intro/tls_mutual_auth/cluster_tls.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
