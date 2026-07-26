---
title: "Search store"
source: "https://stalw.art/docs/storage/fts"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/fts"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Search store"
---
# Search store

      

Full-text search (FTS) indexes message bodies, attachments, contacts, and calendar events so that search queries return quickly across large mailboxes. The backend used for full-text indexing is configured independently from the data store, allowing the search engine to be chosen to match query volume, language requirements, and operational constraints.

Backend selection is made on the [[ref/object/search-store|SearchStore]] singleton (found in the WebUI under  Settings ›  Storage › Search Store). The object is a multi-variant type. The supported variants are:

[[storage/backends/elasticsearch|ElasticSearch]]: a distributed, RESTful search engine.
[[storage/backends/meilisearch|Meilisearch]]: a fast, relevance-focused search engine with minimal configuration.
The `Default` variant, which uses the configured data store as the full-text index. Supported for all data store variants, it stores indexes directly in the database using bloom filters.
FoundationDB, PostgreSQL, and MySQL may also be selected as dedicated search backends when their indexing characteristics are preferred.

## Choosing a search store

[Section titled “Choosing a search store”](#choosing-a-search-store)

The internal (data-store-backed) full-text index offers several advantages:

Privacy: indexes are kept in the database using bloom filters, a space-efficient probabilistic data structure that allows membership testing without exposing indexed tokens directly. While bloom filters do not encrypt data, they obscure individual terms.
Automatic multi-language support: message language is detected before indexing, and a language-appropriate analyser is applied.
Simplicity: no additional services are required.

External engines offer their own strengths:

Advanced text analysis and relevance scoring: synonyms, custom analysers, and sophisticated ranking algorithms.
Ranking and sorting flexibility: more control over how results are ordered, combining text relevance with structured fields.
Storage efficiency: purpose-built index structures designed for full-text search.

### Limitations

[Section titled “Limitations”](#limitations)

Every FTS backend involves trade-offs. No single engine is optimal for every deployment; each backend makes design choices that affect scalability, accuracy, resource usage, and query expressiveness. Administrators should consider these in the context of expected mail volume, query patterns, and operational constraints.

The following table summarises the primary limitations of each supported engine:

Full Text Search EngineKey LimitationsElasticsearchMemory intensive and Java based, which can increase operational complexity and resource usage.MeilisearchSingle-node architecture and [[storage/backends/meilisearch#limitations|limited support]] for complex, deeply nested query expressions.PostgreSQLMaximum indexable email content size of approximately [[storage/backends/postgresql#fts-limitations|650 KB per message]].MySQL[[storage/backends/mysql#fts-limitations|No stemming support]] and no support for indexing multiple languages within the same column.Internal FTSHigher write amplification compared to external full text search backends.

These limitations are not defects but consequences of architectural choices. If search accuracy, language support, scalability, or operational simplicity is a critical requirement, evaluating multiple backends may be necessary.

## Configuration

[Section titled “Configuration”](#configuration)

To change the search backend, update the SearchStore singleton and select the appropriate variant. Variant-specific fields such as [[ref/object/search-store#url|`url`]], [[ref/object/search-store#httpauth|`httpAuth`]], [[ref/object/search-store#numshards|`numShards`]], and [[ref/object/search-store#numreplicas|`numReplicas`]] apply to the ElasticSearch variant; [[ref/object/search-store#url|`url`]], [[ref/object/search-store#pollinterval|`pollInterval`]], and [[ref/object/search-store#maxretries|`maxRetries`]] apply to the Meilisearch variant. See the [[ref/object/search-store|SearchStore reference]] for the full field list per variant.

### Index configuration

[Section titled “Index configuration”](#index-configuration)

Which entities are indexed, and which of their fields, is controlled through the [[ref/object/search|Search]] object (found in the WebUI under  Settings ›  Search). Indexing for each entity type can be enabled or disabled independently through [[ref/object/search#indexemail|`indexEmail`]], [[ref/object/search#indexcontacts|`indexContacts`]], [[ref/object/search#indexcalendar|`indexCalendar`]], and [[ref/object/search#indextelemetry|`indexTelemetry`]].

The fields indexed per entity are controlled through [[ref/object/search#indexemailfields|`indexEmailFields`]], [[ref/object/search#indexcontactfields|`indexContactFields`]], [[ref/object/search#indexcalendarfields|`indexCalendarFields`]], and [[ref/object/search#indextracingfields|`indexTracingFields`]]. Each accepts a list of field names; if the list is empty, all supported fields for that entity are registered.

### Default language

[Section titled “Default language”](#default-language)

When language detection fails (for example, when the text is too short or ambiguous), the server falls back to the language configured in [[ref/object/search#defaultlanguage|`defaultLanguage`]] on the Search object. The default is `"en_US"`. The set of languages considered by the detector can be restricted through [[ref/object/search#supportedlanguages|`supportedLanguages`]].