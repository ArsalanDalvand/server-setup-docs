---
title: "Command key specifications"
source: "https://redis.io/docs/latest/develop/reference/key-specs/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/reference/key-specs"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Command key specifications"
---
# Command key specifications

What are command key specification and how to use them in your client

Many of the commands in Redis accept key names as input arguments.
The 9th element in the reply of [`COMMAND`](/docs/latest/commands/command/) (and [`COMMAND INFO`](/docs/latest/commands/command-info/)) is an array that consists of the command's key specifications.

A key specification describes a rule for extracting the names of one or more keys from the arguments of a given command.
Key specifications provide a robust and flexible mechanism, compared to the first key, last key and step scheme employed until Redis 7.0.
Before introducing these specifications, Redis clients had no trivial programmatic means to extract key names for all commands.

Cluster-aware Redis clients had to have the keys' extraction logic hard-coded in the cases of commands such as [`EVAL`](/docs/latest/commands/eval/) and [`ZUNIONSTORE`](/docs/latest/commands/zunionstore/) that rely on a numkeys argument or [`SORT`](/docs/latest/commands/sort/) and its many clauses.
Alternatively, the [`COMMAND GETKEYS`](/docs/latest/commands/command-getkeys/) can be used to achieve a similar extraction effect but at a higher latency.

A Redis client isn't obligated to support key specifications.
It can continue using the legacy first key, last key and step scheme along with the [movablekeys flag](/docs/latest/commands/command/#flags) that remain unchanged.

However, a Redis client that implements key specifications support can consolidate most of its keys' extraction logic.
Even if the client encounters an unfamiliar type of key specification, it can always revert to the [`COMMAND GETKEYS`](/docs/latest/commands/command-getkeys/) command.

That said, most cluster-aware clients only require a single key name to perform correct command routing, so it is possible that although a command features one unfamiliar specification, its other specification may still be usable by the client.

Key specifications are maps with the following keys:

begin_search:: the starting index for keys' extraction.
find_keys: the rule for identifying the keys relative to the BS.
notes: notes about this key spec, if there are any.
flags: indicate the type of data access.

## begin_search

The begin_search value of a specification informs the client of the extraction's beginning.
The value is a map.
There are three types of `begin_search`:

index: key name arguments begin at a constant index.
keyword: key names start after a specific keyword (token).
unknown: an unknown type of specification - see the [incomplete flag section](#incomplete) for more details.

### index

The index type of `begin_search` indicates that input keys appear at a constant index.
It is a map under the spec key with a single key:

index: the 0-based index from which the client should start extracting key names.

### keyword

The keyword type of `begin_search` means a literal token precedes key name arguments.
It is a map under the spec with two keys:

keyword: the keyword (token) that marks the beginning of key name arguments.
startfrom: an index to the arguments array from which the client should begin searching.
This can be a negative value, which means the search should start from the end of the arguments' array, in reverse order.
For example, -2's meaning is to search reverse from the penultimate argument.

More examples of the keyword search type include:

[`SET`](/docs/latest/commands/set/) has a `begin_search` specification of type index with a value of 1.
[`XREAD`](/docs/latest/commands/xread/) has a `begin_search` specification of type keyword with the values "STREAMS" and 1 as keyword and startfrom, respectively.
[`MIGRATE`](/docs/latest/commands/migrate/) has a start_search specification of type keyword with the values of "KEYS" and -2.

## find_keys

The `find_keys` value of a key specification tells the client how to continue the search for key names.
`find_keys` has three possible types:

range: keys stop at a specific index or relative to the last argument.
keynum: an additional argument specifies the number of input keys.
unknown: an unknown type of specification - see the [incomplete flag section](#incomplete) for more details.

### range

The range type of `find_keys` is a map under the spec key with three keys:

lastkey: the index, relative to `begin_search`, of the last key argument.
This can be a negative value, in which case it isn't relative.
For example, -1 indicates to keep extracting keys until the last argument, -2 until one before the last, and so on.
keystep: the number of arguments that should be skipped, after finding a key, to find the next one.
limit: if lastkey is has the value of -1, we use the limit to stop the search by a factor.
0 and 1 mean no limit.
2 means half of the remaining arguments, 3 means a third, and so on.

### keynum

The keynum type of `find_keys` is a map under the spec key with three keys:

keynumidx: the index, relative to `begin_search`, of the argument containing the number of keys.
firstkey: the index, relative to `begin_search`, of the first key.
This is usually the next argument after keynumidx, and its value, in this case, is greater by one.
keystep: Tthe number of arguments that should be skipped, after finding a key, to find the next one.

Examples:

The [`SET`](/docs/latest/commands/set/) command has a range of 0, 1 and 0.
The [`MSET`](/docs/latest/commands/mset/) command has a range of -1, 2 and 0.
The [`XREAD`](/docs/latest/commands/xread/) command has a range of -1, 1 and 2.
The [`ZUNION`](/docs/latest/commands/zunion/) command has a start_search type index with the value 1, and `find_keys` of type keynum with values of 0, 1 and 1.

Note:
this isn't a perfect solution as the module writers can come up with anything.
However, this mechanism should allow the extraction of key name arguments for the vast majority of commands.

## notes

Notes about non-obvious key specs considerations, if applicable.

## flags

A key specification can have additional flags that provide more details about the key.
These flags are divided into three groups, as described below.

### Access type flags

The following flags declare the type of access the command uses to a key's value or its metadata.
A key's metadata includes LRU/LFU counters, type, and cardinality.
These flags do not relate to the reply sent back to the client.

Every key specification has precisely one of the following flags:

RW: the read-write flag.
The command modifies the data stored in the value of the key or its metadata.
This flag marks every operation that isn't distinctly a delete, an overwrite, or read-only.
RO: the read-only flag.
The command only reads the value of the key (although it doesn't necessarily return it).
OW: the overwrite flag.
The command overwrites the data stored in the value of the key.
RM: the remove flag.
The command deletes the key.

### Logical operation flags

The following flags declare the type of operations performed on the data stored as the key's value and its TTL (if any), not the metadata.
These flags describe the logical operation that the command executes on data, driven by the input arguments.
The flags do not relate to modifying or returning metadata (such as a key's type, cardinality, or existence).

Every key specification may include the following flag:

access: the access flag.
This flag indicates that the command returns, copies, or somehow uses the user's data that's stored in the key.

In addition, the specification may include precisely one of the following:

update: the update flag.
The command updates the data stored in the key's value.
The new value may depend on the old value.
This flag marks every operation that isn't distinctly an insert or a delete.
insert: the insert flag.
The command only adds data to the value; existing data isn't modified or deleted.
delete: the delete flag.
The command explicitly deletes data from the value stored at the key.

### Miscellaneous flags

Key specifications may have the following flags:

not_key: this flag indicates that the specified argument isn't a key.
This argument is treated the same as a key when computing which slot a command should be assigned to for Redis cluster.
For all other purposes this argument should not be considered a key.
incomplete: this flag is explained below.
variable_flags: this flag is explained below.

### incomplete

Some commands feature exotic approaches when it comes to specifying their keys, which makes extraction difficult.
Consider, for example, what would happen with a call to [`MIGRATE`](/docs/latest/commands/migrate/) that includes the literal string "KEYS" as an argument to its AUTH clause.
Our key specifications would miss the mark, and extraction would begin at the wrong index.

Thus, we recognize that key specifications are incomplete and may fail to extract all keys.
However, we assure that even incomplete specifications never yield the wrong names of keys, providing that the command is syntactically correct.

In the case of [`MIGRATE`](/docs/latest/commands/migrate/), the search begins at the end (startfrom has the value of -1).
If and when we encounter a key named "KEYS", we'll only extract the subset of the key name arguments after it.
That's why [`MIGRATE`](/docs/latest/commands/migrate/) has the incomplete flag in its key specification.

Another case of incompleteness is the [`SORT`](/docs/latest/commands/sort/) command.
Here, the `begin_search` and `find_keys` are of type unknown.
The client should revert to calling the [`COMMAND GETKEYS`](/docs/latest/commands/command-getkeys/) command to extract key names from the arguments, short of implementing it natively.
The difficulty arises, for example, because the string "STORE" is both a keyword (token) and a valid literal argument for [`SORT`](/docs/latest/commands/sort/).

Note:
the only commands with incomplete key specifications are [`SORT`](/docs/latest/commands/sort/) and [`MIGRATE`](/docs/latest/commands/migrate/).
We don't expect the addition of such commands in the future.

### variable_flags

In some commands, the flags for the same key name argument can depend on other arguments.
For example, consider the [`SET`](/docs/latest/commands/set/) command and its optional  GET argument.
Without the GET argument, [`SET`](/docs/latest/commands/set/) is write-only, but it becomes a read and write command with it.
When this flag is present, it means that the key specification flags cover all possible options, but the effective flags depend on other arguments.

## Examples

### SET key specifications

```
1) 1) "flags"
     2) 1) RW
        2) access
        3) update
     3) "begin_search"
     4) 1) "type"
        2) "index"
        3) "spec"
        4) 1) "index"
           2) (integer) 1
     5) "find_keys"
     6) 1) "type"
        2) "range"
        3) "spec"
        4) 1) "lastkey"
           2) (integer) 0
           3) "keystep"
           4) (integer) 1
           5) "limit"
           6) (integer) 0
```

### ZUNION key specifications

```
1) 1) "flags"
     2) 1) RO
        2) access
     3) "begin_search"
     4) 1) "type"
        2) "index"
        3) "spec"
        4) 1) "index"
           2) (integer) 1
     5) "find_keys"
     6) 1) "type"
        2) "keynum"
        3) "spec"
        4) 1) "keynumidx"
           2) (integer) 0
           3) "firstkey"
           4) (integer) 1
           5) "keystep"
           6) (integer) 1
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page