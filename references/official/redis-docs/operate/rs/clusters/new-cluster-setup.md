---
title: "Set up a new cluster"
source: "https://redis.io/docs/latest/operate/rs/clusters/new-cluster-setup/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/new-cluster-setup"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Set up a new cluster"
---
# Set up a new cluster

How to set up a new cluster using the management UI.

            Redis Software
          

A Redis Software cluster typically consists of several nodes.
For production deployments, we recommend an uneven number of nodes, with a minimum of three.

Note:
  In a cluster that consists of only one node, some features and capabilities are not enabled,
such as database replication that provides high availability.

To set up a new cluster, you must first [[operate/rs/installing-upgrading|install the Redis Software package]]
and then set up the cluster as described below.
After the cluster is created you can [add multiple nodes to the cluster](/docs/latest/operate/rs/clusters/add-node/).

To create a cluster:

In a browser, go to `https://<name or IP address of the machine with Redis Software installed>:8443`.
For example, if you installed Redis SoftwareÂ on a machine with IP address 10.0.1.34, go to [https://10.0.1.34:8443](https://10.0.1.34:8443).

Note:

TheÂ management UI uses a [self-signed certificate for TLS encryption](/docs/latest/operate/rs/security/certificates/updating-certificates/).
If the machine has both an internal IP address and an external IP address, use the external IP address to access the setup UI.

Select Create new cluster.

Enter an email and password for the administrator account, then select Next to proceed to cluster setup.

Enter your cluster license key if you have one. Otherwise, the cluster uses the trial license by default.

In the Configuration section:

For FQDN (Fully Qualified Domain Name), enter a unique name for the cluster.

See the [[operate/rs/networking/cluster-dns|instructions for DNS setup]]
to make sure your cluster is reachable by name.

Choose whether to [[operate/rs/networking/private-public-endpoints|Enable private & public endpoints support]].

Choose whether to [[operate/rs/clusters/configure/rack-zone-awareness|Enable rack-zone awareness]].

Click Next.

Configure storage and network settings:

Enter a path forÂ [[operate/rs/installing-upgrading/install/plan-deployment/persistent-ephemeral-storage|Ephemeral storage]], or leave the default path.

Enter a path forÂ [[operate/rs/installing-upgrading/install/plan-deployment/persistent-ephemeral-storage|Persistent storage]],
or leave the default path.

To enable [Redis Flex or Auto Tiering](/docs/latest/operate/rs/databases/flash/),
select Enable flash storage and enter the path to the flash storage.

If the cluster is configured to support [[operate/rs/clusters/configure/rack-zone-awareness|rack-zone awareness]], set the Rack-zone ID for the new node.

If your machine has multiple IP addresses, assign a single IPv4 type address for Node-to-node communication (internal traffic) and multiple IPv4/IPv6 type addresses for External traffic.

Select Create cluster.

Click OK to confirm that you are aware of the replacement of the HTTPS TLS certificate on the node,
and proceed through the browser warning.

After a short wait, your cluster is created and you can sign in to the Cluster Manager UI.

You can now access any of the management capabilities, including:

[Creating a new database](/docs/latest/operate/rs/databases/create/)
[Joining a new node to a cluster](/docs/latest/operate/rs/clusters/add-node/)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)