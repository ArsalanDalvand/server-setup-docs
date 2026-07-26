---
title: "Technical Overview"
source: "https://apple.github.io/foundationdb/technical-overview.html"
source_format: official-rst
language: "en"
doc_slug: "technical-overview"
category: "foundationdb"
retrieved_at: "2026-07-26T14:40:29+00:00"
tags:
  - foundationdb
  - official-documentation
  - server-architecture
aliases:
  - "Technical Overview"
---
##################
Technical Overview
##################

These documents explain the engineering design of FoundationDB, with detailed information on its features, architecture, and performance.

* [[jozve/server-setup-docs/references/official/foundationdb-docs/architecture|architecture]] provides a diagrammatic description of FoundationDB's logical architecture.

* [[layer-concept|layer-concept]]: Applications use layers to add capabilities to FoundationDB's minimal key-value API. A layer can provide a new data model, encapsulate an algorithm, or even be an entire framework.

* [[transaction-processing|transaction-processing]]: FoundationDB supports transaction processing over a distributed cluster. We use optimistic concurrency control with a transactional authority running on a small number of transaction servers.

* [[jozve/server-setup-docs/references/official/foundationdb-docs/performance|performance]] describes the scaling, latency, throughput, and concurrency behavior of FoundationDB under various workloads and configurations.

* [[benchmarking|benchmarking]] describes how we approach testing FoundationDB using different client concurrencies and cluster sizes.

* [[jozve/server-setup-docs/references/official/foundationdb-docs/features|features]] describes the fundamental properties of FoundationDB, as well as those that relate to performance, concurrency, and operations.

* [[anti-features|anti-features]]: What a system *isn't* is sometimes as important as what it is. FoundationDB explicitly excludes some features either because they are at odds with the overall system design, or because they are better implemented as [[layer-concept|layers]].

* [[experimental-features|experimental-features]] describes experimental features of FoundationDB, available for testing.

* [[jozve/server-setup-docs/references/official/foundationdb-docs/engineering|engineering]] summarizes the unique tools and capabilities behind FoundationDB.

* [[fault-tolerance|fault-tolerance]]: FoundationDB provides fault tolerance by intelligently replicating data across a distributed cluster of machines. Our architecture is designed to minimize service interruption and data loss in the event of machine failures.

* [[flow|flow]]: FoundationDB faces rigorous engineering challenges for high performance and scalability. To meet these challenges, we implemented Flow, an extension to C++ that supports actor-based concurrency with new keywords and control-flow primitives while retaining speed and I/O efficiency.

* [[jozve/server-setup-docs/references/official/foundationdb-docs/testing|testing]]: FoundationDB uses a combined regime of robust simulation, live performance testing, and hardware-based failure testing to meet exacting standards of correctness and performance.

* [[kv-architecture|kv-architecture]] provides a description of every major role a process in FoundationDB can fulfill.

* [[read-write-path|read-write-path]] describes how FDB read and write path works.

* [[ha-write-path|ha-write-path]] describes how FDB write path works in HA setting.

* [[consistency-check-urgent|consistency-check-urgent]] describes how to complete a consistency scan of the entire database in a fast way.

.. toctree::
   :maxdepth: 1
   :titlesonly:
   :hidden:

   architecture
   performance
   benchmarking
   engineering
   features
   layer-concept
   anti-features
   experimental-features
   transaction-processing
   fault-tolerance
   flow
   testing
   kv-architecture
   read-write-path
   ha-write-path
   consistency-check-urgent
