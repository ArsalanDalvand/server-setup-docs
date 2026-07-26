---
title: "Pipelines and transactions"
source: "https://redis.io/docs/latest/develop/clients/redis-py/transpipe/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/clients/redis-py/transpipe"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Pipelines and transactions"
---
# Pipelines and transactions

Learn how to use Redis pipelines and transactions

Redis lets you send a sequence of commands to the server together in a batch.
There are two types of batch that you can use:

Pipelines avoid network and processing overhead by sending several commands
to the server together in a single communication. The server then sends back
a single communication with all the responses. See the
[[develop/using-commands/pipelining|Pipelining]] page for more
information.
Transactions guarantee that all the included commands will execute
to completion without being interrupted by commands from other clients.
See the [[develop/using-commands/transactions|Transactions]]
page for more information.

## Execute a pipeline

To execute commands in a pipeline, you first create a pipeline object
and then add commands to it using methods that resemble the standard
command methods (for example, `set()` and `get()`). The commands are
buffered in the pipeline and only execute when you call the `execute()`
method on the pipeline object. This method returns a list that contains
the results from all the commands in order.

Note that the command methods for a pipeline always return the original
pipeline object, so you can "chain" several commands together, as the
example below shows:

Language:

                        Python
                    

Foundational: Use pipelines to batch multiple commands together and reduce network round trips

```python
r = redis.Redis(decode_responses=True)

pipe = r.pipeline()

for i in range(5):
    pipe.set(f"seat:{i}", f"#{i}")

set_5_result = pipe.execute()
print(set_5_result)  # >>> [True, True, True, True, True]

pipe = r.pipeline()

# "Chain" pipeline commands together.
get_3_result = pipe.get("seat:0").get("seat:3").get("seat:4").execute()
print(get_3_result)  # >>> ['#0', '#3', '#4']
```

```python

```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/trans_pipe.py)

## Execute a transaction

A pipeline actually executes as a transaction by default (that is to say,
all commands are executed in an uninterrupted sequence). However, if you
need to switch this behavior off, you can set the `transaction` parameter
to `False` when you create the pipeline:

```python
pipe = r.pipeline(transaction=False)
```

## Watch keys for changes

Redis supports optimistic locking to avoid inconsistent updates
to different keys. The basic idea is to watch for changes to any
keys that you use in a transaction while you are processing the
updates. If the watched keys do change, you must restart the updates
with the latest data from the keys. See
[[develop/using-commands/transactions|Transactions]]
for more information about optimistic locking.

The example below shows how to repeatedly attempt a transaction with a watched
key until it succeeds. The code reads a string
that represents a `PATH` variable for a command shell, then appends a new
command path to the string before attempting to write it back. If the watched
key is modified by another client before writing, the transaction aborts
with a `WatchError` exception, and the loop executes again for another attempt.
Otherwise, the loop terminates successfully.

Language:

                        Python
                    

Optimistic locking: Use WATCH to monitor keys for changes and retry transactions when conflicts occur

```python
r.set("shellpath", "/usr/syscmds/")

with r.pipeline() as pipe:
    # Repeat until successful.
    while True:
        try:
            # Watch the key we are about to change.
            pipe.watch("shellpath")

            # The pipeline executes commands directly (instead of
            # buffering them) from immediately after the `watch()`
            # call until we begin the transaction.
            current_path = pipe.get("shellpath")
            new_path = current_path + ":/usr/mycmds/"

            # Start the transaction, which will enable buffering
            # again for the remaining commands.
            pipe.multi()

            pipe.set("shellpath", new_path)

            pipe.execute()

            # The transaction succeeded, so break out of the loop.
            break
        except redis.WatchError:
            # The transaction failed, so continue with the next attempt.
            continue

get_path_result = r.get("shellpath")
print(get_path_result)  # >>> '/usr/syscmds/:/usr/mycmds/'
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/trans_pipe.py)

Because this is a common pattern, the library includes a convenience
method called `transaction()` that handles the code to watch keys,
execute the transaction, and retry if necessary. Pass
`transaction()` a function that implements your main transaction code,
and also pass the keys you want to watch. The example below implements
the same basic transaction as the previous example but this time
using `transaction()`. Note that `transaction()` can't add the `multi()`
call automatically, so you must still place this correctly in your
transaction function.

Language:

                        Python
                    

Optimistic locking with convenience method: Use the transaction() helper to simplify watched transaction patterns

```python
r.set("shellpath", "/usr/syscmds/")

def watched_sequence(pipe):
    current_path = pipe.get("shellpath")
    new_path = current_path + ":/usr/mycmds/"

    pipe.multi()

    pipe.set("shellpath", new_path)

trans_result = r.transaction(watched_sequence, "shellpath")
print(trans_result)  # True

get_path_result = r.get("shellpath")
print(get_path_result)  # >>> '/usr/syscmds/:/usr/mycmds/'
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/trans_pipe.py)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page