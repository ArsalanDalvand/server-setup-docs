---
title: "Configuration Management"
source: "https://docs.nats.io/running-a-nats-service/configuration/resource_management/configuration_mgmt.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/resource_management/configuration_mgmt"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Configuration Management"
---
# Configuration Management

In many cases managing the configuration in your application code is the best model, many teams though wish to pre-create Streams and Consumers.

We support a number of tools to assist with this:

* [CLI with Configuration Files](/running-a-nats-service/configuration/resource_management/configuration_mgmt/nats-admin-cli.md)
* [Terraform](/running-a-nats-service/configuration/resource_management/configuration_mgmt/terraform.md)
* [GitHub Actions](/running-a-nats-service/configuration/resource_management/configuration_mgmt/github_actions.md)
* [Kubernetes JetStream Controller](/running-a-nats-service/configuration/resource_management/configuration_mgmt/kubernetes_controller.md)


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/resource_management/configuration_mgmt.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
