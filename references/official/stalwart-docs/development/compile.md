---
title: "Compiling Stalwart"
source: "https://stalw.art/docs/development/compile"
source_format: official-html-to-markdown
language: "en"
doc_slug: "development/compile"
category: "development"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Compiling Stalwart"
---
# Compiling Stalwart

      

Compiling Stalwart from the source has the main advantage that binaries are built with optimizations specific to your hardware which may result in better performance.
Another advantage is that you may enable or disable features to suit your needs, thus reducing the size of the binary and the memory footprint.

## Install Rust

[Section titled “Install Rust”](#install-rust)

To compile Stalwart you need the latest version of [Rust](https://www.rust-lang.org/). The fastest way to install rust is with `rustup`. On Unix systems run the following in your terminal, then follow the onscreen instructions

Terminal window

```
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

If you are running Windows 64-bit, download and run [rustup‑init.exe](https://rustup.rs), then follow the onscreen instructions.

## Clone the repository

[Section titled “Clone the repository”](#clone-the-repository)

Clone the Stalwart repository:

Terminal window

```
$ git clone https://github.com/stalwartlabs/stalwart.git$ cd stalwart
```

Find and then check out the latest tagged release, for example:

Terminal window

```
$ git tag --sort=-version:refname | head -1v0.16.0$ git checkout tags/v0.16.0
```

## Install any required dependencies

[Section titled “Install any required dependencies”](#install-any-required-dependencies)

If you are compiling the Foundation DB backend, download and install the [Foundation DB client library](https://github.com/apple/foundationdb/releases).

## Compile

[Section titled “Compile”](#compile)

For example, to compile Stalwart with the RocksDB, ElasticSearch support and Enterprise [features](#available-features):

Terminal window

```
$ cargo build --release -p stalwart --no-default-features --features "rocks elastic redis enterprise"
```

Or, to compile the FoundationDB backend:

Terminal window

```
$ cargo build --release -p stalwart  --no-default-features --features "foundationdb enterprise"
```

Once the compilation process is completed, the Stalwart binary will be available under `target/release/stalwart`.

## Available Features

[Section titled “Available Features”](#available-features)

You can enable the following features when compiling Stalwart by using the `--features` flag:

### Data store backends

[Section titled “Data store backends”](#data-store-backends)

`rocks`: RocksDB backend
`sqlite`: SQLite backend
`foundationdb`: FoundationDB backend
`postgres`: PostgreSQL backend
`mysql`: MySQL backend

### Blob store backends

[Section titled “Blob store backends”](#blob-store-backends)

`s3`: S3 backend
`azure`: Azure backend

### FTS backends

[Section titled “FTS backends”](#fts-backends)

`elastic`: ElasticSearch backend

### In-memory backends

[Section titled “In-memory backends”](#in-memory-backends)

`redis`: Redis backend

### Message queue backends

[Section titled “Message queue backends”](#message-queue-backends)

`nats`: NATS backend
`zenoh`: Eclipse Zenoh backend
`kafka`: Kafka backend

### Other features

[Section titled “Other features”](#other-features)

`enterprise`: Enterprise features