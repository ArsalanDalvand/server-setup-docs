---
title: "Overview"
source: "https://stalw.art/docs/management/cli"
source_format: official-html-to-markdown
language: "en"
doc_slug: "management/cli"
category: "management"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Overview"
---
# Overview

      

`stalwart-cli` is a small, schema-driven command line tool for administering a Stalwart Mail and Collaboration server. It speaks exclusively to the JMAP API and operates on the management/configuration objects (those whose JMAP type names are prefixed with `x:`) exposed via the `urn:stalwart:jmap` capability.

The tool is schema-driven: on first use it downloads (and caches) a JSON schema from the server that describes every object, its fields, the forms used to render it, and the lists used to render queries. All command behaviour, validation, and human-readable output is derived from that schema, so the same binary works against any compatible Stalwart deployment without recompilation.

## Capabilities at a glance

[Section titled “Capabilities at a glance”](#capabilities-at-a-glance)
TaskCommandTitleInspect what objects and enums the server exposes`describe`[[management/cli/describe|Exploring the schema]]Read a single object by id (or a singleton)`get`[[management/cli/get|Fetching a single object]]List or filter objects of a given type`query`[[management/cli/query|Searching and listing]]Create one or more objects`create`[[management/cli/create|Creating objects]]Modify an existing object (or singleton)`update`[[management/cli/update|Updating objects]]Destroy one or more objects`delete`[[management/cli/delete|Removing objects]]Apply a bulk plan from a JSON file`apply`[[management/cli/apply|Declarative bulk operations]]Export live server state as an apply plan`snapshot`[[management/cli/snapshot|Exporting server state]]

## Installation

[Section titled “Installation”](#installation)

Pre-built binaries are produced for macOS (Intel and Apple Silicon), Linux (x86_64 and aarch64, glibc and musl), and Windows (x86_64 and aarch64), alongside a multi-architecture Docker image. Pick the installer that matches the platform.

### macOS / Linux (shell installer)

[Section titled “macOS / Linux (shell installer)”](#macos--linux-shell-installer)
Terminal window

```
curl --proto '=https' --tlsv1.2 -LsSf \  https://github.com/stalwartlabs/cli/releases/latest/download/stalwart-cli-installer.sh | sh
```

### Windows (PowerShell installer)

[Section titled “Windows (PowerShell installer)”](#windows-powershell-installer)
Terminal window

```
powershell -ExecutionPolicy Bypass -c "irm https://github.com/stalwartlabs/cli/releases/latest/download/stalwart-cli-installer.ps1 | iex"
```

### Homebrew (macOS / Linux)

[Section titled “Homebrew (macOS / Linux)”](#homebrew-macos--linux)
Terminal window

```
brew install stalwartlabs/tap/stalwart-cli
```

### npm (Node.js, all platforms)

[Section titled “npm (Node.js, all platforms)”](#npm-nodejs-all-platforms)
Terminal window

```
npm install -g @stalwartlabs/cli
```

### Docker (all platforms)

[Section titled “Docker (all platforms)”](#docker-all-platforms)

A multi-architecture image (`linux/amd64` and `linux/arm64`) is published with every release to both GitHub Container Registry and Docker Hub:

Terminal window

```
docker run --rm ghcr.io/stalwartlabs/cli --versiondocker run --rm stalwartlabs/cli --version
```

Image tags track releases: a full version (`:1.2.3`), the minor series (`:1.2`), and `:latest`. Images are signed with [cosign](https://docs.sigstore.dev/cosign/overview/) and carry build-provenance attestations.

Pass connection settings through the environment, and bind-mount a working directory for commands that read or write files (`apply --file`, `snapshot --output`):

Terminal window

```
docker run --rm \  -e STALWART_URL=https://mail.example.com \  -e STALWART_TOKEN \  ghcr.io/stalwartlabs/cli query Account --fields id,name
docker run --rm \  -v "$PWD:/work" -w /work \  -e STALWART_URL -e STALWART_TOKEN \  ghcr.io/stalwartlabs/cli apply --file plan.json
```

The container runs as a non-root user whose home is `/home/nonroot`. To persist the [schema cache](#schema-cache) across runs (and avoid a re-fetch each invocation), mount a volume at `/home/nonroot/.cache`:

Terminal window

```
docker run --rm \  -v stalwart-cli-cache:/home/nonroot/.cache \  -e STALWART_URL -e STALWART_TOKEN \  ghcr.io/stalwartlabs/cli describe
```

### Windows MSI

[Section titled “Windows MSI”](#windows-msi)

A signed `.msi` installer is published with each release at
`https://github.com/stalwartlabs/cli/releases/latest`. Download and run it for a standard Windows install.

### Building from source

[Section titled “Building from source”](#building-from-source)

With a recent Rust toolchain (edition 2024):

Terminal window

```
git clone https://github.com/stalwartlabs/cli.gitcd clicargo install --path .
```

Verify the install with:

Terminal window

```
stalwart-cli --version
```

## Connecting to a server

[Section titled “Connecting to a server”](#connecting-to-a-server)

Every invocation needs a server URL and a credential. The same flags are global and may appear before or after the subcommand.

FlagEnvironment variablePurpose`--url <URL>``STALWART_URL`The Stalwart server endpoint (required).`--user <USER>``STALWART_USER`Basic-auth username.`--password <PASS>``STALWART_PASSWORD`Basic-auth password. Prompted interactively if absent and stdin is a TTY.`--api-key <TOKEN>``STALWART_TOKEN`Bearer token. Mutually exclusive with `--user`.`--insecure` / `-k`(none)Skip TLS certificate verification (matches `curl -k`).`--no-color`(none)Disable ANSI color output. The `NO_COLOR` environment variable also disables colors.

A typical invocation against a local development server:

Terminal window

```
stalwart-cli --url http://localhost:8080 --user admin --password admin describe
```

Equivalent with environment variables (recommended for scripts and CI):

Terminal window

```
export STALWART_URL=https://mail.example.comexport STALWART_USER=adminexport STALWART_PASSWORD='changeme'stalwart-cli describe
```

For unattended use, prefer an API token:

Terminal window

```
export STALWART_URL=https://mail.example.comexport STALWART_TOKEN='eyJhbGciOi...'stalwart-cli describe
```

## Object naming

[Section titled “Object naming”](#object-naming)

The CLI accepts object names with or without the `x:` prefix and is case-insensitive. The following are all equivalent:

Terminal window

```
stalwart-cli get Domain abc123stalwart-cli get domain abc123stalwart-cli get x:Domain abc123stalwart-cli get X:DOMAIN abc123
```

Internally, names are normalised to the canonical form before any JMAP request leaves the client. In all human-readable output, the `x:` prefix is stripped (so `x:Domain` is shown as `Domain`).

## Singletons

[Section titled “Singletons”](#singletons)

Some configuration objects are singletons: there is exactly one instance per server. Singletons accept (or default to) the literal id `singleton`. For example, the JMAP limits singleton can be fetched with either of these:

Terminal window

```
stalwart-cli get jmapstalwart-cli get jmap singleton
```

Singletons cannot be created or destroyed, only updated. The [[management/cli/update|Updating objects]] and [[management/cli/get|Fetching a single object]] pages cover the specifics.

## Multi-variant objects

[Section titled “Multi-variant objects”](#multi-variant-objects)

Some objects (for example `Account`) have multiple variants discriminated by an `@type` field (`User`, `Group`, etc.). When [[management/cli/create|creating]] such an object, two equivalent forms are accepted:

The shorthand `Object/Variant` syntax (`stalwart-cli create account/user ...`), which auto-injects `@type`.
Passing `@type` explicitly via `--field` or as part of a `--json` payload.

For [[management/cli/get|`get`]], [[management/cli/query|`query`]], [[management/cli/update|`update`]], and [[management/cli/delete|`delete`]], only the bare object name is accepted (the slash form is rejected).

## Output formats

[Section titled “Output formats”](#output-formats)

Read commands support `--json` for machine-readable output. Each output record is a single compact JSON line. Commands that produce a single record (`get --json`) emit one line. Commands that produce a stream (`query --json`, `apply --json`, `snapshot`) emit NDJSON (newline-delimited JSON): one complete record per line, no enclosing array. This format streams (constant memory regardless of result-set size) and pipes naturally into `jq`, `mlr`, `awk`, and similar tools. To consume an NDJSON stream as a single JSON array, pipe through `jq -s '.'`.

Without `--json`, the CLI renders a human-friendly view driven by the schema’s form definitions, with ANSI color when stdout is a TTY.

Color is automatically disabled when:

stdout is not a TTY (for example, when piping to `head`, `less`, or a file).
The `NO_COLOR` environment variable is set.
`--no-color` is passed explicitly.

## Schema cache

[Section titled “Schema cache”](#schema-cache)

The schema is fetched from `/api/schema` and cached locally under the standard XDG cache directory:

Linux: `~/.cache/stalwart-cli/<server-hash>/`
macOS: `~/Library/Caches/stalwart-cli/<server-hash>/`
Windows: `%LOCALAPPDATA%\stalwart-cli\<server-hash>\`

`<server-hash>` is a URL-safe base64 of the SHA-256 of the normalised server URL, so multiple Stalwart deployments coexist in the cache without collision. The cache is consulted before each command. If the server hash hasn’t changed, no schema download is issued. If the network is unreachable but a cached copy exists, the CLI emits a warning and proceeds with the cached schema.

## Errors

[Section titled “Errors”](#errors)

All errors are written to stderr in the form `error: <message>` and the process exits with a non-zero status. JMAP-level errors are surfaced with their `type` and `description`. Per-object set errors (validation failures, primary key violations, dependent-object protection, etc.) include the affected properties and any linked-object information returned by the server.

Piping output to a short-lived consumer such as `head` or `less` is handled cleanly: a broken pipe is treated as a normal end of output, not as an error.

## Getting help

[Section titled “Getting help”](#getting-help)

Standard help is available for every subcommand:

Terminal window

```
stalwart-cli --helpstalwart-cli get --helpstalwart-cli apply --help
```

For schema-driven help (descriptions of objects, fields, enums, filters, and sort options), use the [[management/cli/describe|`describe`]] command.