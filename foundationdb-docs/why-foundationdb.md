---
title: "Why FoundationDB"
source: "https://apple.github.io/foundationdb/why-foundationdb.html"
source_format: official-rst
language: "en"
doc_slug: "why-foundationdb"
category: "foundationdb"
retrieved_at: "2026-07-26T14:40:29+00:00"
tags:
  - foundationdb
  - official-documentation
  - server-architecture
aliases:
  - "Why FoundationDB"
---
################
Why FoundationDB
################

NoSQL database design involves a number of fundamental technical alternatives. Among these alternatives, transactions yields significant advantages. FoundationDB is designed to perform transaction processing with high performance at scale.

* [[transaction-manifesto|transaction-manifesto]]: Full ACID transactions are a powerful tool with significant advantages. To develop applications using modern distributed databases, transactions are simply essential. 

* The [[cap-theorem|cap-theorem]] has been widely discussed in connection with NoSQL databases and is often invoked to justify weak models of data consistency. The truth is that the CAP theorem is widely misunderstood based on imprecise descriptions and actually constrains system design far less than is usually supposed.

* [[consistency|consistency]] is fundamental to the design of distributed databases. However, the term *consistency* is used with two different meanings in discussions of the ACID properties and the CAP theorem, and it's easy to confuse them.

* [[scalability|scalability]]: FoundationDB is horizontally scalable, achieving high performance per node in configurations from a single machine to a large cluster. We also offer elasticity, allowing machines to be provisioned or deprovisioned in a running cluster with automated load balancing and data distribution.

.. toctree::
   :maxdepth: 1
   :titlesonly:
   :hidden:

   transaction-manifesto
   cap-theorem
   consistency
   scalability
