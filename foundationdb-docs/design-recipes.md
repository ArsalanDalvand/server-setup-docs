---
title: "Design Recipes"
source: "https://apple.github.io/foundationdb/design-recipes.html"
source_format: official-rst
language: "en"
doc_slug: "design-recipes"
category: "foundationdb"
retrieved_at: "2026-07-26T14:40:29+00:00"
tags:
  - foundationdb
  - official-documentation
  - server-architecture
aliases:
  - "Design Recipes"
---
##############
Design Recipes
##############

Learn how to build new data models, indexes, and more on top of the FoundationDB API. For more background, check out the [[client-design|client-design]] documentation.

* [[blob|Blob]]: Store binary large objects (blobs) in the database.

* [[hierarchical-documents|Hierarchical Documents]]: Create a representation for hierarchical documents.

* [[multimaps|Multimaps]]: Create a multimap data structure with multiset values.

* [[priority-queues|Priority Queues]]: Create a data structure for priority queues supporting operations for push, pop_min, peek_min, pop_max, and peek_max.

* [[queues|Queues]]: Create a queue data structure that supports FIFO operations.

* [[segmented-range-reads|Segmented Range Reads]]: Perform range reads in calibrated batches.

* [[simple-indexes|Simple Indexes]]: Add (one or more) indexes to allow efficient retrieval of data in multiple ways.

* [[spatial-indexing|Spatial Indexing]]: Create a spatial index for the database.

* [[subspace-indirection|Subspace Indirection]]: Employ subspace indirection to manage bulk inserts or similar long-running operations.

* [[tables|Tables]]: Create a table data structure suitable for sparse data.

* [[vector|Vector]]: Create a vector data structure.

.. toctree::
     :maxdepth: 1
     :titlesonly:
     :hidden:

     blob
     blob-java
     hierarchical-documents
     hierarchical-documents-java
     multimaps
     multimaps-java
     priority-queues
     priority-queues-java
     queues
     queues-java
     segmented-range-reads
     segmented-range-reads-java
     simple-indexes
     simple-indexes-java
     spatial-indexing
     spatial-indexing-java
     subspace-indirection
     subspace-indirection-java
     tables
     tables-java
     vector
     vector-java

