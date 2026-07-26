---
title: "Removing objects"
source: "https://stalw.art/docs/management/cli/delete"
source_format: official-html-to-markdown
language: "en"
doc_slug: "management/cli/delete"
category: "management"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Removing objects"
---
# Removing objects

      

The `delete` command destroys one or more objects by id. Singletons cannot be destroyed (they can only be updated; see [[management/cli/update|Updating objects]]).

## Synopsis

[Section titled “Synopsis”](#synopsis)

```
stalwart-cli delete <object> ( --ids id1,id2,... | --stdin )
```

`<object>`: object type name (with or without `x:` prefix). The slash form is rejected.
`--ids`: comma-separated list of ids.
`--stdin`: read ids from standard input.

Exactly one of `--ids` and `--stdin` must be supplied.

## Direct ids

[Section titled “Direct ids”](#direct-ids)
Terminal window

```
stalwart-cli delete domain --ids xyz789stalwart-cli delete domain --ids id1,id2,id3
```

Whitespace around commas is tolerated. Empty entries are ignored.

## Reading ids from stdin

[Section titled “Reading ids from stdin”](#reading-ids-from-stdin)

`--stdin` accepts either a JSON array of strings:

Terminal window

```
echo '["id1","id2","id3"]' | stalwart-cli delete domain --stdin
```

or a comma-, space-, or newline-separated list:

Terminal window

```
printf 'id1\nid2\nid3\n' | stalwart-cli delete domain --stdin
```

This is convenient when piping from [[management/cli/query|`query`]]:

Terminal window

```
stalwart-cli query domain --where namespace=tmp --json \  | stalwart-cli delete domain --stdin
```

(Replace `--json` with the appropriate jq filter as needed: `... --json | jq -r '.[] | .id' | xargs -d, stalwart-cli delete domain --ids` for an alternate flow.)

## Output

[Section titled “Output”](#output)

One line per id, then a summary:

```
id1 deletedid2 deletedid3 failed: objectIsLinked  Linked by:   DkimSignature#abc, DkimSignature#def2 deleted, 1 failed
```

The exit status is `0` if every deletion succeeded and non-zero if at least one failed. Failed entries report the JMAP `SetError` type, the linked objects (when applicable), and any validation details.

## Batching

[Section titled “Batching”](#batching)

Internally, deletions are split into batches of `maxObjectsInSet` (taken from the JMAP session, typically 500). Large deletes are streamed without any extra effort from the caller.

## Common failures

[Section titled “Common failures”](#common-failures)
SetError typeMeaningResolution`objectIsLinked`Other objects reference this oneDestroy the linked objects first (the message lists them)`notFound`The id does not existConfirm with [[management/cli/query|`query`]]`forbidden`Insufficient permissions on this object typeAuthenticate as a user with the required permission`singleton`An attempt was made to destroy a singletonUse [[management/cli/update|`update`]] instead

For dependency-aware bulk teardowns (where order matters), the [[management/cli/apply|`apply`]] command processes destroys in reverse plan order, which makes it the right tool for tearing down full deployments.

## See also

[Section titled “See also”](#see-also)

[[management/cli/query|Searching and listing]] to obtain the ids to delete.
[[management/cli/apply|Declarative bulk operations]] for filter-based destroys and dependency-ordered teardowns.