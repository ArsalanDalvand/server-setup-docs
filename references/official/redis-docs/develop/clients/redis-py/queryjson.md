---
title: "Index and query documents"
source: "https://redis.io/docs/latest/develop/clients/redis-py/queryjson/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/clients/redis-py/queryjson"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Index and query documents"
---
# Index and query documents

Learn how to use Redis Search with JSON and hash documents.

This example shows how to create a
[search index](/docs/latest/develop/ai/search-and-query/indexing/)
for [JSON](/docs/latest/develop/data-types/json/) documents and
run queries against the index. It then goes on to show the slight differences
in the equivalent code for [hash](/docs/latest/develop/data-types/hashes/)
documents.

Note:
  From [v6.0.0](https://github.com/redis/redis-py/releases/tag/v6.0.0) onwards,
`redis-py` uses query dialect 2 by default.
Redis Search methods such as [`ft().search()`](/docs/latest/commands/ft.search/)
will explicitly request this dialect, overriding the default set for the server.
See
[Query dialects](/docs/latest/develop/ai/search-and-query/advanced-concepts/dialects/)
for more information.

## Initialize

Make sure that you have [[operate/oss_and_stack|Redis Open Source]]
or another Redis server available. Also install the
[[develop/clients/redis-py|`redis-py`]] client library if you
haven't already done so.

Add the following dependencies. All of them are applicable to both JSON and hash,
except for the `Path` class, which is specific to JSON (see
[Path](/docs/latest/develop/data-types/json/path/) for a description of the
JSON path syntax).

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Foundational: Import required libraries for Redis Search, JSON operations, and search functionality

Ctrl+Enter to run

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

## Create data

Create some test data to add to your database. The example data shown
below is compatible with both JSON and hash objects.

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Foundational: Define sample user data structures for indexing and querying

Ctrl+Enter to run

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

## Add the index

Connect to your Redis database. The code below shows the most
basic connection but see
[[develop/clients/redis-py/connect|Connect to the server]]
to learn more about the available connection options.

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Foundational: Establish a connection to a Redis server for query operations

Ctrl+Enter to run

```python
r = redis.Redis(decode_responses=True)
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

The example uses an index called `idx:users` for JSON documents and adds
some JSON documents with the `user:` key prefix. To avoid errors, first
delete any existing index or documents whose names that might
conflict with the example:

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Foundational: Clean up existing indexes and documents to prepare for fresh example data

Ctrl+Enter to run

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

Create an index for the JSON data. The code below specifies that only JSON documents with
the key prefix `user:` are indexed. For more information, see
[Query syntax](/docs/latest/develop/ai/search-and-query/query/).

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Foundational: Create a search index for JSON documents with field definitions and key prefix filtering

Ctrl+Enter to run

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

## Add the data

Add the three sets of user data to the database as
[JSON](/docs/latest/develop/data-types/json/) objects.
If you use keys with the `user:` prefix then Redis will index the
objects automatically as you add them:

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Foundational: Store JSON documents in Redis with automatic indexing based on key prefix

Ctrl+Enter to run

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

## Query the data

You can now use the index to search the JSON objects. The
[query](/docs/latest/develop/ai/search-and-query/query/)
below searches for objects that have the text "Paul" in any field
and have an `age` value in the range 30 to 40:

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Query with filters: Search JSON documents using text matching and numeric range filters to find specific records

Ctrl+Enter to run

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

Specify query options to return only the `city` field:

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Query with field projection: Retrieve only specific fields from search results to reduce data transfer

Ctrl+Enter to run

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

Use an
[aggregation query](/docs/latest/develop/ai/search-and-query/query/aggregation/)
to count all users in each city.

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Aggregation queries: Use GROUP BY and COUNT operations to summarize and analyze indexed data

Ctrl+Enter to run

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

## Differences with hash documents

Indexing for hash documents is very similar to JSON indexing but you
need to specify some slightly different options.

When you create the schema for a hash index, you don't need to
add aliases for the fields, since you use the basic names to access
the fields anyway. Also, you must use `HASH` for the `IndexType`
when you create the index.

First delete any existing index or documents
whose names might conflict with the hash example:

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Foundational: Clean up existing hash indexes and documents to prepare for fresh example data

Ctrl+Enter to run

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

Create a new index called `hash-idx:users`, which is otherwise the same as
the `idx:users` index used for JSON documents in the previous examples:

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Foundational: Create a search index for hash documents with HASH index type and field definitions

Ctrl+Enter to run

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

You use [`hset()`](/docs/latest/commands/hset/) to add the hash
documents instead of [`json().set()`](/docs/latest/commands/json.set/),
but the same flat `userX` dictionaries work equally well with either
hash or JSON:

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Foundational: Store hash documents in Redis with automatic indexing based on key prefix

Ctrl+Enter to run

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

The query commands work the same here for hash as they do for JSON (but
the name of the hash index is different). The format of the result is
almost the same except that the fields are returned directly in the
result `Document` object instead of in an enclosing `json` dictionary:

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-py_home_json?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Query with filters: Search hash documents using text matching and numeric range filters (same as JSON queries)

Ctrl+Enter to run

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
"""
JSON examples from redis-py "home" page"
 https://redis.io/docs/latest/develop/connect/clients/python/redis-py/#example-indexing-and-querying-json-documents
"""
```

```python
import redis
from redis.commands.json.path import Path
import redis.commands.search.aggregation as aggregations
import redis.commands.search.reducers as reducers
from redis.commands.search.field import TextField, NumericField, TagField
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
import redis.exceptions
```

```python
user1 = {
    "name": "Paul John",
    "email": "[email protected]",
    "age": 42,
    "city": "London"
}

user2 = {
    "name": "Eden Zamir",
    "email": "[email protected]",
    "age": 29,
    "city": "Tel Aviv"
}

user3 = {
    "name": "Paul Zamir",
    "email": "[email protected]",
    "age": 35,
    "city": "Tel Aviv"
}
```

```python
r = redis.Redis(decode_responses=True)
```

```python
try:
    r.ft("idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("user:1", "user:2", "user:3")
```

```python
schema = (
    TextField("$.name", as_name="name"),
    TagField("$.city", as_name="city"),
    NumericField("$.age", as_name="age")
)

indexCreated = r.ft("idx:users").create_index(
    schema,
    definition=IndexDefinition(
        prefix=["user:"], index_type=IndexType.JSON
    )
)
```

```python
user1Set = r.json().set("user:1", Path.root_path(), user1)
user2Set = r.json().set("user:2", Path.root_path(), user2)
user3Set = r.json().set("user:3", Path.root_path(), user3)
```

```python
findPaulResult = r.ft("idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulResult)
# >>> Result{1 total, docs: [Document {'id': 'user:3', ...
```

```python
citiesResult = r.ft("idx:users").search(
    Query("Paul").return_field("$.city", as_field="city")
).docs

print(citiesResult)
# >>> [Document {'id': 'user:1', 'payload': None, ...
```

```python
req = aggregations.AggregateRequest("*").group_by(
    '@city', reducers.count().alias('count')
)

aggResult = r.ft("idx:users").aggregate(req).rows
print(aggResult)
# >>> [['city', 'London', 'count', '1'], ['city', 'Tel Aviv', 'count', '2']]
```

```python
try:
    r.ft("hash-idx:users").dropindex(True)
except redis.exceptions.ResponseError:
    pass

r.delete("huser:1", "huser:2", "huser:3")
```

```python
hashSchema = (
    TextField("name"),
    TagField("city"),
    NumericField("age")
)

hashIndexCreated = r.ft("hash-idx:users").create_index(
    hashSchema,
    definition=IndexDefinition(
        prefix=["huser:"], index_type=IndexType.HASH
    )
)
```

```python
huser1Set = r.hset("huser:1", mapping=user1)
huser2Set = r.hset("huser:2", mapping=user2)
huser3Set = r.hset("huser:3", mapping=user3)
```

```python
findPaulHashResult = r.ft("hash-idx:users").search(
    Query("Paul @age:[30 40]")
)

print(findPaulHashResult)
# >>> Result{1 total, docs: [Document {'id': 'huser:3',
# >>>   'payload': None, 'name': 'Paul Zamir', ...
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

## More information

See the [Redis Search](/docs/latest/develop/ai/search-and-query/) docs
for a full description of all query features with examples.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page