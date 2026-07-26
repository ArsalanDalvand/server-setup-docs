---
title: "Redis command arguments"
source: "https://redis.io/docs/latest/develop/reference/command-arguments/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/reference/command-arguments"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis command arguments"
---
# Redis command arguments

How Redis commands expose their documentation programmatically

The [`COMMAND DOCS`](/docs/latest/commands/command-docs/) command returns documentation-focused information about available Redis commands.
The map reply that the command returns includes the arguments key.
This key stores an array that describes the command's arguments.

Every element in the arguments array is a map with the following fields:

name: the argument's name, always present.
The name of an argument is given for identification purposes alone.
It isn't displayed during the command's syntax rendering.
The same name can appear more than once in the entire argument tree, but it is unique compared to other sibling arguments' names.
This allows obtaining a unique identifier for each argument (the concatenation of all names in the path from the root to any argument).
display_text: the argument's display string, present in arguments that have a displayable representation (all arguments that aren't oneof/block).
This is the string used in the command's syntax rendering.
type: the argument's type, always present.
An argument must have one of the following types:

string: a string argument.
integer: an integer argument.
double: a double-precision argument.
key: a string that represents the name of a key.
pattern: a string that represents a glob-like pattern.
unix-time: an integer that represents a Unix timestamp.
pure-token: an argument is a token, meaning a reserved keyword, which may or may not be provided.
Not to be confused with free-text user input.
oneof: the argument is a container for nested arguments.
This type enables choice among several nested arguments (see the [`XADD`](/docs/latest/commands/xadd/) example below).
block: the argument is a container for nested arguments.
This type enables grouping arguments and applying a property (such as optional) to all (see the [`XADD`](/docs/latest/commands/xadd/) example below).

key_spec_index: this value is available for every argument of the key type.
It is a 0-based index of the specification in the command's [key specifications](/develop/reference/key-specs.md) that corresponds to the argument.
token: a constant literal that precedes the argument (user input) itself.
summary: a short description of the argument.
since: the debut Redis version of the argument (or for module commands, the module version).
deprecated_since: the Redis version that deprecated the command (or for module commands, the module version).
flags: an array of argument flags.
Possible flags are:

optional: denotes that the argument is optional (for example, the GET clause of the  [`SET`](/docs/latest/commands/set/) command).
multiple: denotes that the argument may be repeated (such as the key argument of [`DEL`](/docs/latest/commands/del/)).
multiple-token: denotes the possible repetition of the argument with its preceding token (see [`SORT`](/docs/latest/commands/sort/)'s `GET pattern` clause).

value: the argument's value.
For arguments types other than oneof and block, this is a string that describes the value in the command's syntax.
For the oneof and block types, this is an array of nested arguments, each being a map as described in this section.

## Example

The trimming clause of [`XADD`](/docs/latest/commands/xadd/), i.e., `[MAXLEN|MINID [=|~] threshold [LIMIT count]]`, is represented at the top-level as block-typed argument.

It consists of four nested arguments:

trimming strategy: this nested argument has an oneof type with two nested arguments.
Each of the nested arguments, MAXLEN and MINID, is typed as pure-token.
trimming operator: this nested argument is an optional oneof type with two nested arguments.
Each of the nested arguments, = and ~, is a pure-token.
threshold: this nested argument is a string.
count: this nested argument is an optional integer with a token (LIMIT).

Here's [`XADD`](/docs/latest/commands/xadd/)'s arguments array:

```
1) 1) "name"
   2) "key"
   3) "type"
   4) "key"
   5) "value"
   6) "key"
2)  1) "name"
    2) "nomkstream"
    3) "type"
    4) "pure-token"
    5) "token"
    6) "NOMKSTREAM"
    7) "since"
    8) "6.2"
    9) "flags"
   10) 1) optional
3) 1) "name"
   2) "trim"
   3) "type"
   4) "block"
   5) "flags"
   6) 1) optional
   7) "value"
   8) 1) 1) "name"
         2) "strategy"
         3) "type"
         4) "oneof"
         5) "value"
         6) 1) 1) "name"
               2) "maxlen"
               3) "type"
               4) "pure-token"
               5) "token"
               6) "MAXLEN"
            2) 1) "name"
               2) "minid"
               3) "type"
               4) "pure-token"
               5) "token"
               6) "MINID"
               7) "since"
               8) "6.2"
      2) 1) "name"
         2) "operator"
         3) "type"
         4) "oneof"
         5) "flags"
         6) 1) optional
         7) "value"
         8) 1) 1) "name"
               2) "equal"
               3) "type"
               4) "pure-token"
               5) "token"
               6) "="
            2) 1) "name"
               2) "approximately"
               3) "type"
               4) "pure-token"
               5) "token"
               6) "~"
      3) 1) "name"
         2) "threshold"
         3) "type"
         4) "string"
         5) "value"
         6) "threshold"
      4)  1) "name"
          2) "count"
          3) "type"
          4) "integer"
          5) "token"
          6) "LIMIT"
          7) "since"
          8) "6.2"
          9) "flags"
         10) 1) optional
         11) "value"
         12) "count"
4) 1) "name"
   2) "id_or_auto"
   3) "type"
   4) "oneof"
   5) "value"
   6) 1) 1) "name"
         2) "auto_id"
         3) "type"
         4) "pure-token"
         5) "token"
         6) "*"
      2) 1) "name"
         2) "id"
         3) "type"
         4) "string"
         5) "value"
         6) "id"
5) 1) "name"
   2) "field_value"
   3) "type"
   4) "block"
   5) "flags"
   6) 1) multiple
   7) "value"
   8) 1) 1) "name"
         2) "field"
         3) "type"
         4) "string"
         5) "value"
         6) "field"
      2) 1) "name"
         2) "value"
         3) "type"
         4) "string"
         5) "value"
         6) "value"
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page