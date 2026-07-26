---
title: "redis-py guide (Python)"
source: "https://redis.io/docs/latest/develop/clients/redis-py/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/clients/redis-py"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-py guide (Python)"
---
# redis-py guide (Python)

Connect your Python application to a Redis database

[redis-py](https://github.com/redis/redis-py) is the Python client for Redis.
The sections below explain how to install `redis-py` and connect your application
to a Redis database.

`redis-py` requires a running Redis server. See [[operate/oss_and_stack/install|here]] for Redis Open Source installation instructions.

You can also access Redis with an object-mapping client interface. See
[[integrate/redisom-for-python|RedisOM for Python]]
for more information.

## Install

To install `redis-py`, enter:

```bash
pip install redis
```

For faster performance, install Redis with [`hiredis`](https://github.com/redis/hiredis) support. This provides a compiled response parser, and for most cases requires zero code changes. By default, if `hiredis` >= 1.0 is available, `redis-py` attempts to use it for response parsing.

Note:
  The Python `distutils` packaging scheme is no longer part of Python 3.12 and greater. If you're having difficulties getting `redis-py` installed in a Python 3.12 environment, consider updating to a recent release of `redis-py`.

```bash
pip install redis[hiredis]
```

## Connect and test

Connect to localhost on port 6379, set a value in Redis, and retrieve it. All responses are returned as bytes in Python. To receive decoded strings, set `decode_responses=True`. For more connection options, see [these examples](https://redis.readthedocs.io/en/stable/examples.html).

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-landing?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Connect to a Redis server and establish a client connection

Ctrl+Enter to run

```python
import redis
r = redis.Redis(host='localhost', port=6379, decode_responses=True)
```

```python
import redis
r = redis.Redis(host='localhost', port=6379, decode_responses=True)
```

```python
r.set('foo', 'bar')
# True
r.get('foo')
# bar
```

```python
r.hset('user-session:123', mapping={
    'name': 'John',
    "surname": 'Smith',
    "company": 'Redis',
    "age": 29
})
# True

r.hgetall('user-session:123')
# {'surname': 'Smith', 'name': 'John', 'company': 'Redis', 'age': '29'}
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

Store and retrieve a simple string.

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-landing?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Set and retrieve string values using SET and GET commands

Ctrl+Enter to run

```python
r.set('foo', 'bar')
# True
r.get('foo')
# bar
```

```python
import redis
r = redis.Redis(host='localhost', port=6379, decode_responses=True)
```

```python
r.set('foo', 'bar')
# True
r.get('foo')
# bar
```

```python
r.hset('user-session:123', mapping={
    'name': 'John',
    "surname": 'Smith',
    "company": 'Redis',
    "age": 29
})
# True

r.hgetall('user-session:123')
# {'surname': 'Smith', 'name': 'John', 'company': 'Redis', 'age': '29'}
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

Store and retrieve a dict.

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-landing?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Store and retrieve hash data structures using HSET and HGETALL

Ctrl+Enter to run

```python
r.hset('user-session:123', mapping={
    'name': 'John',
    "surname": 'Smith',
    "company": 'Redis',
    "age": 29
})
# True

r.hgetall('user-session:123')
# {'surname': 'Smith', 'name': 'John', 'company': 'Redis', 'age': '29'}
```

```python
import redis
r = redis.Redis(host='localhost', port=6379, decode_responses=True)
```

```python
r.set('foo', 'bar')
# True
r.get('foo')
# bar
```

```python
r.hset('user-session:123', mapping={
    'name': 'John',
    "surname": 'Smith',
    "company": 'Redis',
    "age": 29
})
# True

r.hgetall('user-session:123')
# {'surname': 'Smith', 'name': 'John', 'company': 'Redis', 'age': '29'}
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

Close the connection when you're done.

Language:
Python

[Run in browser](https://redis.io/binder/v2/gh/redis/binder-launchers/python-landing?urlpath=%2Fdoc%2Ftree%2Fdemo.ipynb)

Close the Redis connection

Ctrl+Enter to run

```python
r.close()
```

```python
import redis
r = redis.Redis(host='localhost', port=6379, decode_responses=True)
```

```python
r.set('foo', 'bar')
# True
r.get('foo')
# bar
```

```python
r.hset('user-session:123', mapping={
    'name': 'John',
    "surname": 'Smith',
    "company": 'Redis',
    "age": 29
})
# True

r.hgetall('user-session:123')
# {'surname': 'Smith', 'name': 'John', 'company': 'Redis', 'age': '29'}
```

```python
r.close()
```

[[develop/clients/redis-py|Python Quick-Start]]

## More information

The [`redis-py`](https://redis.readthedocs.io/en/stable/index.html) website
has a [command reference](https://redis.readthedocs.io/en/stable/commands.html)
and some [tutorials](https://redis.readthedocs.io/en/stable/examples.html) for
various tasks. There are also some examples in the
[GitHub repository](https://github.com/redis/redis-py) for `redis-py`.

See also the other pages in this section for more information and examples:

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page