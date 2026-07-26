---
title: "Client Design"
source: "https://apple.github.io/foundationdb/client-design.html"
source_format: official-rst
language: "en"
doc_slug: "client-design"
category: "foundationdb"
retrieved_at: "2026-07-26T14:40:29+00:00"
tags:
  - foundationdb
  - official-documentation
  - server-architecture
aliases:
  - "Client Design"
---
#############
Client Design
#############

FoundationDB supports language bindings for application development using the ordered key-value store. The following documents cover use of the bindings, from getting started and design principles to best practices and data modeling. The latest changes are detailed in [[release-notes]].

* [[getting-started-mac|getting-started-mac]] explains how to install a local FoundationDB server suitable for development on macOS.

* [[getting-started-linux|getting-started-linux]] explains how to install a local FoundationDB server suitable for development on Linux.

* [[downloads|downloads]] describes the FoundationDB packages available on our website.

* [[developer-guide|developer-guide]] explains principles of application development applicable across all language bindings.

* [[data-modeling|data-modeling]] explains recommended techniques for representing application data in the key-value store.

* [[client-testing|client-testing]] Explains how one can use workloads to test client code.

* [[api-general|api-general]] contains information on FoundationDB clients applicable across all language bindings.

* [[transaction-tagging|transaction-tagging]] contains information about using transaction tags in your client code to enable targeted transaction throttling.

* [[api-version-upgrade-guide|api-version-upgrade-guide]] contains information about upgrading client code to a new API version.

* [[transaction-profiler-analyzer|transaction-profiler-analyzer]] contains information about enabling transaction profiling and analyzing.

* [[known-limitations|known-limitations]] describes both long-term design limitations of FoundationDB and short-term limitations applicable to the current version.

* [[tenants|tenants]] describes the use of the tenants feature to define named transaction domains.

* [[automatic-idempotency|automatic-idempotency]] describes the use of a transaction option to prevent transactions from failing with ``commit_unknown_result``.

.. toctree::
    :maxdepth: 1
    :titlesonly:
    :hidden:

    getting-started-mac
    getting-started-linux
    downloads
    developer-guide
    data-modeling
    client-testing
    api-general
    transaction-tagging
    known-limitations
    transaction-profiler-analyzer
    api-version-upgrade-guide
    tenants
    automatic-idempotency
