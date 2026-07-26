---
title: "Export"
source: "https://stalw.art/docs/migration/import-export/export"
source_format: official-html-to-markdown
language: "en"
doc_slug: "migration/import-export/export"
category: "migration"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Export"
---
# Export

      

Exporting is the second half of a Vandelay migration: it reads a local archive and reproduces its contents on a target JMAP server such as Stalwart. Export is the only write path to a target, and JMAP is the only target protocol; whatever protocol a source originally spoke, the data was normalised into the JMAP model on the way into the archive, so the export side has a single uniform job to do. A complete migration is therefore an import that fills an archive followed by an export that applies it.

Terminal window

```
vandelay export \  --url <URL> \  (--auth-basic <USER> [--auth-password <PASS>] | --auth-bearer [TOKEN]) \  (--account-id <ID> | --account-name <NAME>) \  [--objects <list>] [--prune [--yes]] \  <ARCHIVE>
```

## Stateless re-matching

[Section titled “Stateless re-matching”](#stateless-re-matching)

Export keeps no per-target state in the archive. Every run is a full, stateless reconciliation: for each data type Vandelay discovers the objects the target already holds, matches each archived object against them using a per-type identity key, and then creates only the objects that matched nothing. The matching keys are based on stable, content-derived identity rather than on server-assigned ids, so they produce the same result on every run. This is what makes a repeated export converge without creating duplicates, and what lets an interrupted export resume simply by being run again: objects already present on the target are recognised and skipped.

Because no synchronisation state is persisted, the in-memory map from archived objects to target objects is rebuilt from scratch on each run. That map is also what resolves references between types. Container types such as mailboxes, calendars and address books are reconciled first, and the items that belong to them are reconciled afterwards, with their parent references resolved through the map built while the containers were processed. Role mailboxes such as Inbox, Sent and Trash are matched by their role rather than by name, so they bind to the target’s server-provisioned folders instead of being recreated.

## Connection, authentication and account

[Section titled “Connection, authentication and account”](#connection-authentication-and-account)

The target server is selected with `--url`, accepting either a base URL or a full JMAP session URL. Exactly one authentication method is required: `--auth-basic <USER>`, with the password resolved from `--auth-password`, the `VANDELAY_PASSWORD` environment variable or a prompt; or `--auth-bearer`, with the token resolved inline, from `VANDELAY_TOKEN`, or from a prompt. Exactly one account selector is required, naming the target account either by its server-assigned identifier with `--account-id` or by its name with `--account-name`. Unlike import, export applies no source-change protection, because it persists no state on the target to protect.

## Type selection

[Section titled “Type selection”](#type-selection)

By default export reconciles every data type that has rows in the archive. The `--objects` flag narrows this to a comma-separated list, which is useful for staging a migration one data type at a time or for re-applying only a subset. If the target server does not advertise the capability for a selected type, that type is reported with a warning and skipped while the rest of the run proceeds.

## Additive default and pruning

[Section titled “Additive default and pruning”](#additive-default-and-pruning)

The default behaviour of export is purely additive. Matched objects are recognised and left in place, unmatched archived objects are created, and any pre-existing objects on the target that the archive does not describe are left untouched. An export therefore never destroys anything on the target unless explicitly told to.

Destructive reconciliation is enabled with `--prune`, which deletes target objects that match nothing in the archive, bringing the target into exact correspondence with the archive rather than merely ensuring the archive’s contents are present. Because this removes data, Vandelay asks for interactive confirmation before proceeding; in automation the prompt can be suppressed with `--yes`. Declining the confirmation aborts the run without making changes. Previewing a prune with `--dry-run` first, to see exactly which objects would be removed, is strongly advisable.

## Examples

[Section titled “Examples”](#examples)

A first export of an archive onto a Stalwart account using Basic authentication:

Terminal window

```
export VANDELAY_PASSWORD='target-password'vandelay export \  --url https://jmap.example.org \  --auth-basic [email protected] \  --account-name [email protected] \  alice.sqlite
```

Exporting only mail and calendar data while a migration is staged in phases:

Terminal window

```
export VANDELAY_PASSWORD='target-password'vandelay export \  --url https://jmap.example.org \  --auth-basic [email protected] \  --account-name [email protected] \  --objects mailbox,email,calendar,calendarevent \  alice.sqlite
```

Bringing a target into exact correspondence with the archive, unattended, by pruning objects the archive does not describe:

Terminal window

```
export VANDELAY_PASSWORD='target-password'vandelay export \  --url https://jmap.example.org \  --auth-basic [email protected] \  --account-name [email protected] \  --prune --yes \  alice.sqlite
```

Each of these runs may be repeated to resume after an interruption or to re-apply an updated archive; the additive default guarantees that repetition without `--prune` only ever adds what is missing.