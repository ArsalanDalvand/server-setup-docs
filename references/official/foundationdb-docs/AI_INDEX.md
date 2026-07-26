---
title: FoundationDB Official Docs — AI Retrieval Index
tags: [foundationdb, ai-index, database, official-documentation]
---

# FoundationDB Official Docs — AI Retrieval Index

> [!info] Semantic Routing Index for AI Agents and RAG Pipelines
> This file acts as the primary conceptual index. It categorizes the 110 offline notes
> and provides descriptive routing paths to prevent context-window bloat during retrieval.

## 📖 How to use
1. **Architecture & Design Questions:** Start with `Core Concepts & Architecture` to understand FDB's multi-version concurrency control (MVCC) and transaction subsystem.
2. **Deployment & Operations:** Refer to `Administration & Operations` for cluster setup, `fdb.cluster` file formats, and Redwood storage engine tuning.
3. **Client Code & APIs:** Check `API References` or the language-specific quickstart tutorials (like `api-python`).

### Core Concepts & Architecture

- [[jozve/server-setup-docs/references/official/foundationdb-docs/architecture|Architecture]] — `architecture.md` (Source: https://apple.github.io/foundationdb/architecture.html)
- [[technical-overview|Technical Overview]] — `technical-overview.md` (Source: https://apple.github.io/foundationdb/technical-overview.html)
- [[transaction-manifesto|ACID transactions]] — `transaction-manifesto.md` (Source: https://apple.github.io/foundationdb/transaction-manifesto.html)
- [[cap-theorem|CAP Theorem]] — `cap-theorem.md` (Source: https://apple.github.io/foundationdb/cap-theorem.html)
- [[consistency|Consistency]] — `consistency.md` (Source: https://apple.github.io/foundationdb/consistency.html)
- [[read-write-path|FDB Read and Write Path]] — `read-write-path.md` (Source: https://apple.github.io/foundationdb/read-write-path.html)
- [[ha-write-path|FDB HA Write Path: How a mutation travels in FDB HA]] — `ha-write-path.md` (Source: https://apple.github.io/foundationdb/ha-write-path.html)
- [[kv-architecture|FoundationDB Architecture]] — `kv-architecture.md` (Source: https://apple.github.io/foundationdb/kv-architecture.html)
- [[redwood|Redwood Storage Engine]] — `redwood.md` (Source: https://apple.github.io/foundationdb/redwood.html)
- [[transaction-processing|Transaction Processing]] — `transaction-processing.md` (Source: https://apple.github.io/foundationdb/transaction-processing.html)
- [[jozve/server-setup-docs/references/official/foundationdb-docs/testing|Simulation and Testing]] — `testing.md` (Source: https://apple.github.io/foundationdb/testing.html)

### Getting Started & Local Development

- [[getting-started-linux|Getting Started on Linux]] — `getting-started-linux.md` (Source: https://apple.github.io/foundationdb/getting-started-linux.html)
- [[getting-started-mac|Getting Started on macOS]] — `getting-started-mac.md` (Source: https://apple.github.io/foundationdb/getting-started-mac.html)
- [[local-dev|begin local development]] — `local-dev.md` (Source: https://apple.github.io/foundationdb/local-dev.html)
- [[downloads|Downloads]] — `downloads.md` (Source: https://apple.github.io/foundationdb/downloads.html)

### Administration & Operations

- [[administration|Administration]] — `administration.md` (Source: https://apple.github.io/foundationdb/administration.html)
- [[jozve/server-setup-docs/references/official/foundationdb-docs/configuration|Configuration]] — `configuration.md` (Source: https://apple.github.io/foundationdb/configuration.html)
- [[moving-a-cluster|Moving a Cluster to New Machines]] — `moving-a-cluster.md` (Source: https://apple.github.io/foundationdb/moving-a-cluster.html)
- [[jozve/server-setup-docs/references/official/foundationdb-docs/tls|Transport Layer Security]] — `tls.md` (Source: https://apple.github.io/foundationdb/tls.html)
- [[monitored-metrics|Monitored Metrics]] — `monitored-metrics.md` (Source: https://apple.github.io/foundationdb/monitored-metrics.html)
- [[fault-tolerance|Fault Tolerance]] — `fault-tolerance.md` (Source: https://apple.github.io/foundationdb/fault-tolerance.html)
- [[scalability|Scalability]] — `scalability.md` (Source: https://apple.github.io/foundationdb/scalability.html)

### Developer Guide & Data Modeling

- [[developer-guide|Developer Guide]] — `developer-guide.md` (Source: https://apple.github.io/foundationdb/developer-guide.html)
- [[data-modeling|Data Modeling]] — `data-modeling.md` (Source: https://apple.github.io/foundationdb/data-modeling.html)
- [[layer-concept|Layer Concept]] — `layer-concept.md` (Source: https://apple.github.io/foundationdb/layer-concept.html)
- [[design-recipes|Design Recipes]] — `design-recipes.md` (Source: https://apple.github.io/foundationdb/design-recipes.html)
- [[client-design|Client Design]] — `client-design.md` (Source: https://apple.github.io/foundationdb/client-design.html)
- [[automatic-idempotency|Automatic Idempotency]] — `automatic-idempotency.md` (Source: https://apple.github.io/foundationdb/automatic-idempotency.html)
- [[special-keys|Special Keys]] — `special-keys.md` (Source: https://apple.github.io/foundationdb/special-keys.html)
- [[jozve/server-setup-docs/references/official/foundationdb-docs/tenants|Tenants]] — `tenants.md` (Source: https://apple.github.io/foundationdb/tenants.html)
- [[jozve/server-setup-docs/references/official/foundationdb-docs/authorization|Authorization]] — `authorization.md` (Source: https://apple.github.io/foundationdb/authorization.html)
- [[request-tracing|Request Tracing]] — `request-tracing.md` (Source: https://apple.github.io/foundationdb/request-tracing.html)

### API References

- [[jozve/server-setup-docs/references/official/foundationdb-docs/api-reference|API Reference]] — `api-reference.md` (Source: https://apple.github.io/foundationdb/api-reference.html)
- [[api-general|Using FoundationDB Clients]] — `api-general.md` (Source: https://apple.github.io/foundationdb/api-general.html)
- [[api-python|Python API]] — `api-python.md` (Source: https://apple.github.io/foundationdb/api-python.html)
- [[api-c|C API]] — `api-c.md` (Source: https://apple.github.io/foundationdb/api-c.html)
- [[api-ruby|Ruby API]] — `api-ruby.md` (Source: https://apple.github.io/foundationdb/api-ruby.html)
- [[api-error-codes|Error Codes]] — `api-error-codes.md` (Source: https://apple.github.io/foundationdb/api-error-codes.html)
- [[api-version-upgrade-guide|API Version Upgrade Guide]] — `api-version-upgrade-guide.md` (Source: https://apple.github.io/foundationdb/api-version-upgrade-guide.html)
- [[client-testing|API Tester]] — `client-testing.md` (Source: https://apple.github.io/foundationdb/client-testing.html)

### Design Patterns & Data Structures (Java/Python)

- [[class-scheduling|Class Scheduling]] — `class-scheduling.md` (Source: https://apple.github.io/foundationdb/class-scheduling.html)
- [[hierarchical-documents|Hierarchical Documents]] — `hierarchical-documents.md` (Source: https://apple.github.io/foundationdb/hierarchical-documents.html)
- [[hierarchical-documents-java|Hierarchical Documents]] — `hierarchical-documents-java.md` (Source: https://apple.github.io/foundationdb/hierarchical-documents-java.html)
- [[multimaps|Multimaps]] — `multimaps.md` (Source: https://apple.github.io/foundationdb/multimaps.html)
- [[multimaps-java|Multimaps]] — `multimaps-java.md` (Source: https://apple.github.io/foundationdb/multimaps-java.html)
- [[priority-queues|Priority Queues]] — `priority-queues.md` (Source: https://apple.github.io/foundationdb/priority-queues.html)
- [[priority-queues-java|Priority Queues]] — `priority-queues-java.md` (Source: https://apple.github.io/foundationdb/priority-queues-java.html)
- [[queues|Queues]] — `queues.md` (Source: https://apple.github.io/foundationdb/queues.html)
- [[queues-java|Queues]] — `queues-java.md` (Source: https://apple.github.io/foundationdb/queues-java.html)
- [[simple-indexes|Simple Indexes]] — `simple-indexes.md` (Source: https://apple.github.io/foundationdb/simple-indexes.html)
- [[simple-indexes-java|Simple Indexes]] — `simple-indexes-java.md` (Source: https://apple.github.io/foundationdb/simple-indexes-java.html)
- [[spatial-indexing|Spatial Indexing]] — `spatial-indexing.md` (Source: https://apple.github.io/foundationdb/spatial-indexing.html)
- [[spatial-indexing-java|Spatial Indexing]] — `spatial-indexing-java.md` (Source: https://apple.github.io/foundationdb/spatial-indexing-java.html)
- [[subspace-indirection|Subspace Indirection]] — `subspace-indirection.md` (Source: https://apple.github.io/foundationdb/subspace-indirection.html)
- [[subspace-indirection-java|Subspace Indirection]] — `subspace-indirection-java.md` (Source: https://apple.github.io/foundationdb/subspace-indirection-java.html)
- [[jozve/server-setup-docs/references/official/foundationdb-docs/tables|Tables]] — `tables.md` (Source: https://apple.github.io/foundationdb/tables.html)
- [[tables-java|Tables]] — `tables-java.md` (Source: https://apple.github.io/foundationdb/tables-java.html)
- [[time-series|Time-Series Data]] — `time-series.md` (Source: https://apple.github.io/foundationdb/time-series.html)

### Release Notes & Supplemental Docs

- [[anti-features|Anti-Features]] — `anti-features.md`
- [[benchmarking|Benchmarking]] — `benchmarking.md`
- [[jozve/server-setup-docs/references/official/foundationdb-docs/blob|Blob]] — `blob.md`
- [[blob-java|Blob]] — `blob-java.md`
- [[consistency-check-urgent|Consistency Checker Urgent]] — `consistency-check-urgent.md`
- [[contents|Site Map]] — `contents.md`
- [[earlier-release-notes|Earlier Release Notes]] — `earlier-release-notes.md`
- [[jozve/server-setup-docs/references/official/foundationdb-docs/engineering|Engineering]] — `engineering.md`
- [[experimental-features|Experimental-Features]] — `experimental-features.md`
- [[jozve/server-setup-docs/references/official/foundationdb-docs/features|Features]] — `features.md`
- [[flow|Flow]] — `flow.md`
- [[global-configuration|Global Configuration]] — `global-configuration.md`
- [[jozve/server-setup-docs/references/official/foundationdb-docs/index|FoundationDB Overview]] — `index.md`
- [[internal-dev-tools|Internal Dev Tools]] — `internal-dev-tools.md`
- [[known-limitations|Known Limitations]] — `known-limitations.md`
- [[largeval|Managing Large Values and Blobs]] — `largeval.md`
- [[jozve/server-setup-docs/references/official/foundationdb-docs/performance|performance]] — `performance.md`
- [[release-notes-014|Release Notes]] — `release-notes/release-notes-014.md`
- [[release-notes-016|Release Notes]] — `release-notes/release-notes-016.md`
- [[release-notes-021|Release Notes]] — `release-notes/release-notes-021.md`
- [[release-notes-022|Release Notes]] — `release-notes/release-notes-022.md`
- [[release-notes-023|Release Notes]] — `release-notes/release-notes-023.md`
- [[release-notes-100|Release Notes]] — `release-notes/release-notes-100.md`
- [[release-notes-200|Release Notes]] — `release-notes/release-notes-200.md`
- [[release-notes-300|Release Notes]] — `release-notes/release-notes-300.md`
- [[release-notes-400|Release Notes]] — `release-notes/release-notes-400.md`
- [[release-notes-410|Release Notes]] — `release-notes/release-notes-410.md`
- [[release-notes-420|Release Notes]] — `release-notes/release-notes-420.md`
- [[release-notes-430|Release Notes]] — `release-notes/release-notes-430.md`
- [[release-notes-440|Release Notes]] — `release-notes/release-notes-440.md`
- [[release-notes-450|Release Notes]] — `release-notes/release-notes-450.md`
- [[release-notes-460|Release Notes]] — `release-notes/release-notes-460.md`
- [[release-notes-500|Release Notes]] — `release-notes/release-notes-500.md`
- [[release-notes-510|Release Notes]] — `release-notes/release-notes-510.md`
- [[release-notes-520|Release Notes]] — `release-notes/release-notes-520.md`
- [[release-notes-600|Release Notes]] — `release-notes/release-notes-600.md`
- [[release-notes-610|Release Notes]] — `release-notes/release-notes-610.md`
- [[release-notes-620|Release Notes]] — `release-notes/release-notes-620.md`
- [[release-notes-630|Release Notes]] — `release-notes/release-notes-630.md`
- [[release-notes-700|Release Notes]] — `release-notes/release-notes-700.md`
- [[release-notes-710|Release Notes]] — `release-notes/release-notes-710.md`
- [[release-notes-720|Release Notes]] — `release-notes/release-notes-720.md`
- [[release-notes-730|Release Notes]] — `release-notes/release-notes-730.md`
- [[segmented-range-reads|Segmented Range Reads]] — `segmented-range-reads.md`
- [[segmented-range-reads-java|Segmented Range Reads]] — `segmented-range-reads-java.md`
- [[transaction-profiler-analyzer|Transaction profiling and analyzing]] — `transaction-profiler-analyzer.md`
- [[transaction-tagging|Transaction Tagging]] — `transaction-tagging.md`
- [[jozve/server-setup-docs/references/official/foundationdb-docs/tutorials|Tutorials]] — `tutorials.md`
- [[vector|Vector]] — `vector.md`
- [[vector-java|Vector]] — `vector-java.md`
- [[visibility|Visibility Documents]] — `visibility.md`
- [[why-foundationdb|Why FoundationDB]] — `why-foundationdb.md`
