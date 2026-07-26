---
title: "Experimental-Features"
source: "https://apple.github.io/foundationdb/experimental-features.html"
source_format: official-rst
language: "en"
doc_slug: "experimental-features"
category: "foundationdb"
retrieved_at: "2026-07-26T14:40:29+00:00"
tags:
  - foundationdb
  - official-documentation
  - server-architecture
aliases:
  - "Experimental-Features"
---
#####################
Experimental-Features
#####################

What is an experimental feature?
================================

Experimental features in FoundationDB are tools or functionalities that developers can use 
to test new ideas or capabilities within the database system. These features are not fully 
developed or officially supported yet, so they should not be used for production.

A feature is flagged "production-ready" when either Apple or Snowflake(or any major contributor)
is using the feature in production.

List of features
----------------

================ =============== ================== ================== ================== 
 Features         Status in 7.0   Status in 7.1      Status in 7.2      Status in 7.3     
================ =============== ================== ================== ================== 
 Redwood          Experimental    Production-ready   Production-ready   Production-ready  
 GetMappedRange                   Experimental       Experimental       Experimental      
 Tenant                           Experimental       Experimental       Experimental      
 MetaCluster                      Experimental       Experimental       Experimental      
 ChangeFeed                       Experimental       Experimental       Experimental      
 Idempotent txn                   Experimental       Experimental       Experimental      
================ =============== ================== ================== ================== 