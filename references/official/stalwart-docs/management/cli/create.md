---
title: "Creating objects"
source: "https://stalw.art/docs/management/cli/create"
source_format: official-html-to-markdown
language: "en"
doc_slug: "management/cli/create"
category: "management"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Creating objects"
---
# Creating objects

      

The `create` command creates a single object from one of four input sources. Validation is intentionally minimal (the server is the source of truth); the CLI only checks that field names exist on the schema and that values are of the right shape.

## Synopsis

[Section titled “Synopsis”](#synopsis)

```
stalwart-cli create <object> ( --field key=value... | --json '<json>' | --file <path> | --stdin )
```

`<object>`: object type name (with or without `x:` prefix). For multi-variant objects, use the `Object/Variant` shorthand to select a variant.
The four input forms are mutually exclusive (an error is raised if more than one is supplied).

## Multi-variant objects

[Section titled “Multi-variant objects”](#multi-variant-objects)

Multi-variant objects (such as `Account`, with variants `User` and `Group`) require an `@type` value. There are two equivalent forms.

### Slash shorthand

[Section titled “Slash shorthand”](#slash-shorthand)

The `Object/Variant` form auto-injects `@type` matching the variant name:

Terminal window

```
stalwart-cli create account/user \  --field name=alice \  --field domainId=b \  --field 'credentials={"0":{"@type":"Password","secret":"hunter2"}}'
```

### Explicit @type

[Section titled “Explicit @type”](#explicit-type)

The same effect can be achieved by supplying `@type` directly:

Terminal window

```
stalwart-cli create account \  --field @type=User \  --field name=alice \  --field domainId=b
```

If both the slash form and an explicit `@type` are supplied, the user’s value wins and a warning is printed. To inspect a multi-variant object’s variants and their fields, see [[management/cli/describe|Exploring the schema]].

## Input sources

[Section titled “Input sources”](#input-sources)

### --field key=value

[Section titled “--field key=value”](#--field-keyvalue)

Repeatable. Sets one top-level property per flag. Values are coerced based on the schema field’s type:

Field typeAccepted forms`boolean``true` / `false` (case-insensitive)`number` (integer)a decimal integer literal`number` (float)any float literal`string`, `enum`, `objectId`, `blobId`, `datetime`the literal as written`object`, `objectList`, `set`, `map`a JSON literal starting with `{` or `[` (quote it for the shell)

Example:

Terminal window

```
stalwart-cli create domain --field name=example.com --field isEnabled=true --field description='Primary domain'
```

For a field whose value is structured (an object or an array), supply explicit JSON:

Terminal window

```
stalwart-cli create dkimsignature \  --field domainId=b \  --field '@type=Dkim1Ed25519Sha256' \  --field 'privateKey={"@type":"Text","secret":"-----BEGIN PRIVATE KEY-----\n..."}' \  --field selector=2026
```

Repeating the same key in one invocation is an error. To set a JSON sub-key in a `create`, use a structured `--field` value (JSON pointer paths are an [[management/cli/update|`update`]] feature, not a `create` feature).

#### Quoting field names

[Section titled “Quoting field names”](#quoting-field-names)

If a field name itself contains characters that would confuse parsing (a literal `=`, for example), wrap it in double or single quotes:

Terminal window

```
stalwart-cli create memorylookupkey \  --field 'namespace=block-list' \  --field '"key=foo=bar"=value'
```

Without quotes, the parser splits on the first `=`. With quotes, the whole quoted string is treated as the key and the next `=` is the separator.

### --json '<json>'

[Section titled “--json '<json>'”](#--json-json)

A complete JSON object passed inline. Useful for one-shot scripts:

Terminal window

```
stalwart-cli create domain \  --json '{"name":"example.org","isEnabled":true,"description":"Primary"}'
```

### --file <path>

[Section titled “--file <path>”](#--file-path)

Reads the JSON object from a file. Convenient for templated payloads (jinja2, jsonnet, …):

Terminal window

```
stalwart-cli create account/user --file users/alice.json
```

### --stdin

[Section titled “--stdin”](#--stdin)

Reads the JSON object from standard input. Plays well with pipelines:

Terminal window

```
jq -n '{name: "ops-team", description: "Ops"}' \  | stalwart-cli create account/group --stdin
```

`--json` / `--file` / `--stdin` accept arbitrarily nested JSON; the slash-in-key restriction only applies to `--field`.

## Output

[Section titled “Output”](#output)

By default, the CLI prints a one-line confirmation including the new server-assigned id:

```
Created Domain xyz789
```

If the server returns additional properties (for example, a generated API key, app password, or DKIM keypair), those are rendered below the confirmation using the same form-driven view as [[management/cli/get|`get`]]. This matters in cases where the value is shown only once and must be captured immediately. The `ApiKey` and `AppPassword` objects do not take an explicit owner field; the credential is attached to the principal that authenticates the CLI request, so a separate `accountId` is neither supplied nor accepted:

Terminal window

```
stalwart-cli create apikey \  --field description='CI deploy key' \  --field 'permissions={"@type":"Inherit"}' \  --field 'allowedIps={}'
```

```
Created ApiKey aaa111
Credential  Secret: API_AAAAEwAAAAHDN7BtUeZSrGrM9DibqfhzRGD7sw
```

The `Secret` value is returned by the server only on this create response. A subsequent [[management/cli/get|`get`]] of the same `ApiKey` returns the server’s masked placeholder (`****`) for `Secret`; the original cannot be recovered. Capture and store the value before running anything else.

Unlike [[management/cli/get|`get`]], `create` does not currently support a JSON output mode (the input flag `--json` is reserved as an input source). For machine-readable workflows, use [[management/cli/apply|`apply`]], which can emit NDJSON status records.

## See also

[Section titled “See also”](#see-also)

[[management/cli/update|Updating objects]] for modifying an existing object.
[[management/cli/apply|Declarative bulk operations]] for creating many objects in one plan, including cross-object references.
[[management/cli/describe|Exploring the schema]] to see required and accepted fields.