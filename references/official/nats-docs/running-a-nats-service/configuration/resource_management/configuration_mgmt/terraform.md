---
title: "Terraform"
source: "https://docs.nats.io/running-a-nats-service/configuration/resource_management/configuration_mgmt/terraform.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/configuration/resource_management/configuration_mgmt/terraform"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Terraform"
---
# Terraform

[Terraform](https://www.terraform.io/) is a Cloud configuration tool from Hashicorp. We maintain a Provider for Terraform called [terraform-provider-jetstream](https://github.com/nats-io/terraform-provider-jetstream/) that can maintain JetStream using Terraform.

Find it in the [Terraform registry](https://registry.terraform.io/providers/nats-io/jetstream/latest/docs).

## Setup

In your project you can configure the Provider like this:

```
provider "jetstream" {
  servers = "connect.ngs.global"
  credentials = "ngs_jetstream_admin.creds"
}
```

Sample code below that creates the `ORDERS` example. Review the [Project README](https://github.com/nats-io/terraform-provider-jetstream#readme) for full details.

```
resource "jetstream_stream" "ORDERS" {
  name     = "ORDERS"
  subjects = ["ORDERS.*"]
  storage  = "file"
  max_age  = 60 * 60 * 24 * 365
}

resource "jetstream_consumer" "ORDERS_NEW" {
  stream_id      = jetstream_stream.ORDERS.id
  durable_name   = "NEW"
  deliver_all    = true
  filter_subject = "ORDERS.received"
  sample_freq    = 100
}

resource "jetstream_consumer" "ORDERS_DISPATCH" {
  stream_id      = jetstream_stream.ORDERS.id
  durable_name   = "DISPATCH"
  deliver_all    = true
  filter_subject = "ORDERS.processed"
  sample_freq    = 100
}

resource "jetstream_consumer" "ORDERS_MONITOR" {
  stream_id        = jetstream_stream.ORDERS.id
  durable_name     = "MONITOR"
  deliver_last     = true
  ack_policy       = "none"
  delivery_subject = "monitor.ORDERS"
}

output "ORDERS_SUBJECTS" {
  value = jetstream_stream.ORDERS.subjects
}
```


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/configuration/resource_management/configuration_mgmt/terraform.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
