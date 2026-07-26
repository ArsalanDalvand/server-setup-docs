---
title: "Enable private andÂ public database endpoints"
source: "https://redis.io/docs/latest/operate/rs/networking/private-public-endpoints/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/networking/private-public-endpoints"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Enable private andÂ public database endpoints"
---
# Enable private andÂ public database endpoints

Describes how to enable public and private endpoints for databases on a cluster.

            Redis Software
          

Each node in Redis Software can be configured with [[operate/rs/networking/multi-ip-ipv6|private and external IP addresses]]. By default, Redis Software databases expose a single endpoint, e.g. cluster.com (FQDN), using the external IP addresses, making it available to the public network (e.g. the internet). Additionally, the cluster can be configured to expose a private FQDN, which utilizes the private IP addresses for access from the private network only (e.g. VPC or an internal network).

When you create a cluster via the UI, you can configure it to expose private and public endpoints.
This is common for environments such as cloud platforms and enterprises.

When doing so, the cluster creates an additional FQDN, e.g. internal.cluster.com for private network (e.g. VPC or an internal network), while the cluster.com FQDN can be used by a public network (e.g. the internet).

You can enable public and private endpoints during cluster creation only.
However, you can still add an additional FQDN in a different domain (cluster.io, for example) after cluster creation.

To enable private and public endpoints:

Verify the IP addresses are bound to the server or instance.

During cluster setup, turn on Enable public endpoints support in the Cluster screen's Configuration section.

[](/docs/latest/images/rs/screenshots/cluster/setup/enable-public-endpoints.png)

If this setting is not enabled when the cluster is created, databases on the cluster support only a single endpoint.

Select Next to proceed to Node configuration.

In the Network configuration section:

Configure the machine's public IP address for external traffic.

Configure the private IP address for both internal and external traffic so it can be used for private database endpoints.

After cluster creation, both sets of endpoints are available for databases in the cluster.

To view and copy public and private endpoints for a database in the cluster, see the database's Configuration > General section.

[](/docs/latest/images/rs/screenshots/databases/config-general-endpoints.png)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)