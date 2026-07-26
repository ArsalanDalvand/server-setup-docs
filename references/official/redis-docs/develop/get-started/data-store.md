---
title: "Redis as an in-memory data structure store quick start guide"
source: "https://redis.io/docs/latest/develop/get-started/data-store/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/get-started/data-store"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis as an in-memory data structure store quick start guide"
---
# Redis as an in-memory data structure store quick start guide

Understand how to use basic Redis data types

This quick start guide shows you how to:

Get started with Redis
Store data under a key in Redis
Retrieve data with a key from Redis
Scan the keyspace for keys that match a specific pattern

The examples in this article refer to a simple bicycle inventory.

## Setup

The easiest way to get started with Redis is to use Redis Cloud:

Create a [free account](https://redis.com/try-free?utm_source=redisio&utm_medium=referral&utm_campaign=2023-09-try_free&utm_content=cu-redis_cloud_users).

Follow the instructions to create a free database.

You can alternatively follow the [[operate/oss_and_stack/install/install-stack|installation guides]] to install Redis on your local machine.

## Connect

The first step is to connect to Redis. You can find further details about the connection options in this documentation site's [[develop/tools|Tools section]]. The following example shows how to connect to a Redis server that runs on localhost (`-h 127.0.0.1`) and listens on the default port (`-p 6379`):

Language:

                        >_ Redis CLI
                    

                        Python
                    

                        JavaScript (node-redis)
                    

                        Java-Sync
                    

                        Go
                    

                        C#-Sync (NRedisStack)
                    

Foundational: Connect to a Redis server

```
> redis-cli -h 127.0.0.1 -p 6379
```

REDIS-CLI

[Redis CLI guide](https://redis.io/docs/latest/develop/tools/cli/)

                            Also, check out our other client tools
                            [Redis Insight](https://redis.io/docs/latest/develop/tools/insight/)
                            and
                            [Redis for VS Code](https://redis.io/docs/latest/develop/tools/redis-for-vscode/).
                        

```python
r = redis.Redis(host="localhost", port=6379, db=0, decode_responses=True)
```

```python

```

REDIS-CLI

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

```node.js
const client = createClient();
client.on('error', err => console.log('Redis Client Error', err));

await client.connect();
```

```node.js

```

REDIS-CLI

[Node.js Quick-Start](https://redis.io/docs/latest/develop/clients/nodejs/)

```java
RedisClient jedis = RedisClient.create("localhost", 6379);
```

```java

```

REDIS-CLI

[Java-Sync Quick-Start](https://redis.io/docs/latest/develop/clients/jedis/)

```go
ctx := context.Background()

	rdb := redis.NewClient(&redis.Options{
		Addr:     "localhost:6379",
		Password: "", // no password docs
		DB:       0,  // use default DB
		Protocol: 2,
	})
```

```go

```

REDIS-CLI

[Go Quick-Start](https://redis.io/docs/latest/develop/clients/go/)

```c#
var muxer = ConnectionMultiplexer.Connect("localhost:6379");
        var db = muxer.GetDatabase();
        var ft = db.FT();
        var json = db.JSON();
```

```c#

```

REDIS-CLI

[C#-Sync (NRedisStack) Quick-Start](https://redis.io/docs/latest/develop/clients/dotnet/)

Tip:
  You can copy and paste the connection details from the Redis Cloud database configuration page. Here is an example connection string of a Cloud database that is hosted in the AWS region `us-east-1` and listens on port 16379: `redis-16379.c283.us-east-1-4.ec2.cloud.redislabs.com:16379`. The connection string has the format `host:port`. You must also copy and paste the username and password of your Cloud database and then either pass the credentials to your client or use the [AUTH command](/docs/latest/commands/auth/) after the connection is established.

## Store and retrieve data

Redis stands for Remote Dictionary Server. You can use the same data types as in your local programming environment but on the server side within Redis.

Similar to byte arrays, Redis strings store sequences of bytes, including text, serialized objects, counter values, and binary arrays. The following example shows you how to set and get a string value:

Language:

                        >_ Redis CLI
                    

                        Python
                    

                        JavaScript (node-redis)
                    

                        JavaScript (ioredis)
                    

                        Java-Sync
                    

                        Java-Async
                    

                        Java-Reactive
                    

                        Go
                    

                        C#-Sync (SE.Redis)
                    

                        PHP
                    

                        Ruby
                    

                        Rust-Sync
                    

                        Rust-Async
                    

Foundational: Set and retrieve string values using SET and GET commands

SET bike:1 "Process 134"
GET bike:1

[Redis CLI guide](https://redis.io/docs/latest/develop/tools/cli/)

                            Also, check out our other client tools
                            [Redis Insight](https://redis.io/docs/latest/develop/tools/insight/)
                            and
                            [Redis for VS Code](https://redis.io/docs/latest/develop/tools/redis-for-vscode/).
                        

```python
"""
Code samples for data structure store quickstart pages:
    https://redis.io/docs/latest/develop/get-started/data-store/
"""

import redis

r = redis.Redis(host="localhost", port=6379, db=0, decode_responses=True)

res = r.set("bike:1", "Process 134")
print(res)
# >>> True

res = r.get("bike:1")
print(res)
# >>> "Process 134"
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/string_set_get.py)

```node.js
import { createClient } from 'redis';

const client = createClient();

client.on('error', err => console.log('Redis Client Error', err));

await client.connect().catch(console.error);

await client.set('bike:1', 'Process 134');
const value = await client.get('bike:1');
console.log(value);
// returns 'Process 134'

await client.close();
```

[Node.js Quick-Start](https://redis.io/docs/latest/develop/clients/nodejs/)

[](https://github.com/redis/node-redis/tree/master/doctests/string-set-get-example.js)

```node.js
import assert from 'node:assert';
import { Redis } from 'ioredis';

const redis = new Redis();

const res1 = await redis.set('bike:1', 'Process 134');
console.log(res1); // >>> OK

const res2 = await redis.get('bike:1');
console.log(res2); // >>> Process 134

redis.disconnect();
```

[ioredis Quick-Start](https://redis.io/docs/latest/develop/clients/ioredis/)

```java
package io.redis.examples;

import redis.clients.jedis.RedisClient;

public class SetGetExample {

  public void run() {

    RedisClient jedis = RedisClient.create("redis://localhost:6379");

    String status = jedis.set("bike:1", "Process 134");

    if ("OK".equals(status)) System.out.println("Successfully added a bike.");

    String value = jedis.get("bike:1");

    if (value != null) System.out.println("The name of the bike is: " + value + ".");

    jedis.close();
  }
}
```

[Java-Sync Quick-Start](https://redis.io/docs/latest/develop/clients/jedis/)

[](https://github.com/redis/jedis/tree/master/src/test/java/io/redis/examples/SetGetExample.java)

```java
package io.redis.examples.async;

import io.lettuce.core.RedisClient;
import io.lettuce.core.api.async.RedisAsyncCommands;
import io.lettuce.core.api.StatefulRedisConnection;

import java.util.concurrent.CompletableFuture;

public class SetGetExample {

    public void run() {
        RedisClient redisClient = RedisClient.create("redis://localhost:6379");

        try (StatefulRedisConnection<String, String> connection = redisClient.connect()) {
            RedisAsyncCommands<String, String> asyncCommands = connection.async();

            CompletableFuture<Void> setGetExample = asyncCommands.set("bike:1", "Process 134")
                    .thenCompose(res1 -> {
                        System.out.println(res1); // >>> OK
                        return asyncCommands.get("bike:1");
                    }).thenAccept(res2 -> {
                        System.out.println(res2); // >>> Process 134
                    }).toCompletableFuture();

            setGetExample.join();
        } finally {
            redisClient.shutdown();
        }
    }
}
```

[Java-Async Quick-Start](https://redis.io/docs/latest/develop/clients/lettuce/)

```java
package io.redis.examples.reactive;

import io.lettuce.core.RedisClient;
import io.lettuce.core.api.reactive.RedisReactiveCommands;
import io.lettuce.core.api.StatefulRedisConnection;

import reactor.core.publisher.Mono;

public class SetGetExample {

    public void run() {
        RedisClient redisClient = RedisClient.create("redis://localhost:6379");

        try (StatefulRedisConnection<String, String> connection = redisClient.connect()) {
            RedisReactiveCommands<String, String> reactiveCommands = connection.reactive();

            Mono<Void> setGetExample = reactiveCommands.set("bike:1", "Process 134")
                    .doOnNext(res1 -> {
                        System.out.println(res1); // >>> OK
                    })
                    .then(reactiveCommands.get("bike:1"))
                    .doOnNext(res2 -> {
                        System.out.println(res2); // >>> Process 134
                    })
                    .then();

            setGetExample.block();
        } finally {
            redisClient.shutdown();
        }
    }
}
```

[Java-Reactive Quick-Start](https://redis.io/docs/latest/develop/clients/lettuce/)

```go
package example_commands_test

import (
	"context"
	"fmt"

	"github.com/redis/go-redis/v9"
)

func ExampleClient_Set_and_get() {
	ctx := context.Background()

	rdb := redis.NewClient(&redis.Options{
		Addr:     "localhost:6379",
		Password: "", // no password docs
		DB:       0,  // use default DB
	})

	err := rdb.Set(ctx, "bike:1", "Process 134", 0).Err()
	if err != nil {
		panic(err)
	}

	fmt.Println("OK")

	value, err := rdb.Get(ctx, "bike:1").Result()
	if err != nil {
		panic(err)
	}
	fmt.Printf("The name of the bike is %s", value)

}
```

[Go Quick-Start](https://redis.io/docs/latest/develop/clients/go/)

[](https://github.com/redis/go-redis/tree/master/doctests/set_get_test.go)

```c#
using NRedisStack.Tests;
using StackExchange.Redis;

public class SetGetExample
{
    public void Run()
    {
        var muxer = ConnectionMultiplexer.Connect("localhost:6379");
        var db = muxer.GetDatabase();

        bool status = db.StringSet("bike:1", "Process 134");

        if (status)
            Console.WriteLine("Successfully added a bike.");

        var value = db.StringGet("bike:1");

        if (value.HasValue)
            Console.WriteLine("The name of the bike is: " + value + ".");

    }
}
```

[C#-Sync (SE.Redis) Quick-Start](https://redis.io/docs/latest/develop/clients/dotnet/)

[](https://github.com/redis/NRedisStack/tree/master/tests/Doc/SetGetExample.cs)

```php
<?php
use Predis\Client as PredisClient;

class SetGetTest
{
    public function testSetGet() {
        $r = new PredisClient([
            'scheme'   => 'tcp',
            'host'     => '127.0.0.1',
            'port'     => 6379,
            'password' => '',
            'database' => 0,
        ]);

        $res1 = $r->set('bike:1', 'Process 134');
        echo $res1 . PHP_EOL; // >>> OK

        $res2 = $r->get('bike:1');
        echo $res2 . PHP_EOL; // >>> Process 134

    }
}
```

[PHP Quick-Start](https://redis.io/docs/latest/develop/clients/php/)

```ruby
require 'redis'

r = Redis.new

res1 = r.set('bike:1', 'Process 134')
puts res1 # >>> OK

res2 = r.get('bike:1')
puts res2 # >>> Process 134
```

[Ruby Quick-Start](https://redis.io/docs/latest/develop/clients/redis-rb/)

```rust
mod set_and_get_tests {
    use redis::Commands;

    fn run() {
        let mut r = match redis::Client::open("redis://127.0.0.1") {
            Ok(client) => match client.get_connection() {
                Ok(conn) => conn,
                Err(e) => {
                    println!("Failed to connect to Redis: {e}");
                    return;
                }
            },
            Err(e) => {
                println!("Failed to create Redis client: {e}");
                return;
            }
        };

        if let Ok(res1) = r.set("bike:1", "Process 134") {
            let res1: String = res1;
            println!("{res1}"); // >>> OK
        }

        if let Ok(res2) = r.get("bike:1") {
            let res2: String = res2;
            println!("{res2}"); // >>> Process 134
        }

    }
}
```

[Rust-Sync Quick-Start](https://redis.io/docs/latest/develop/clients/rust/)

```rust
mod set_and_get_tests {
    use redis::AsyncCommands;

    async fn run() {
        let mut r = match redis::Client::open("redis://127.0.0.1") {
            Ok(client) => match client.get_multiplexed_async_connection().await {
                Ok(conn) => conn,
                Err(e) => {
                    println!("Failed to connect to Redis: {e}");
                    return;
                }
            },
            Err(e) => {
                println!("Failed to create Redis client: {e}");
                return;
            }
        };

        if let Ok(res1) = r.set("bike:1", "Process 134").await {
            let res1: String = res1;
            println!("{res1}"); // >>> OK
        }

        if let Ok(res2) = r.get("bike:1").await {
            let res2: String = res2;
            println!("{res2}"); // >>> Process 134
        }

    }
}
```

[Rust-Async Quick-Start](https://redis.io/docs/latest/develop/clients/rust/)

Hashes are the equivalent of dictionaries (dicts or hash maps). Among other things, you can use hashes to represent plain objects and to store groupings of counters. The following example explains how to set and access field values of an object:

Language:

                        >_ Redis CLI
                    

                        Python
                    

                        JavaScript (node-redis)
                    

                        Java-Sync
                    

                        Java-Async
                    

                        Java-Reactive
                    

                        Go
                    

                        C#-Sync (SE.Redis)
                    

                        PHP
                    

                        Ruby
                    

                        Rust-Sync
                    

                        Rust-Async
                    

Foundational: Store and retrieve hash data structures using HSET to set multiple fields, HGET to retrieve individual fields, and HGETALL to retrieve all fields at once

HSET bike:2 model Deimos brand Ergonom type 'Enduro bikes' price 4972
HGET bike:2 model
HGET bike:2 price
HGETALL bike:2

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

[Redis CLI guide](https://redis.io/docs/latest/develop/tools/cli/)

                            Also, check out our other client tools
                            [Redis Insight](https://redis.io/docs/latest/develop/tools/insight/)
                            and
                            [Redis for VS Code](https://redis.io/docs/latest/develop/tools/redis-for-vscode/).
                        

```python
res1 = r.hset(
    "bike:1",
    mapping={
        "model": "Deimos",
        "brand": "Ergonom",
        "type": "Enduro bikes",
        "price": 4972,
    },
)
print(res1)
# >>> 4

res2 = r.hget("bike:1", "model")
print(res2)
# >>> 'Deimos'

res3 = r.hget("bike:1", "price")
print(res3)
# >>> '4972'

res4 = r.hgetall("bike:1")
print(res4)
# >>> {'model': 'Deimos', 'brand': 'Ergonom', 'type': 'Enduro bikes', 'price': '4972'}
```

```python

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

hset(

name: str,

key: Optional[str] = None,

value: Optional[str] = None,

mapping: Optional[dict] = None,

items: Optional[list] = None

) â 
Union[Awaitable[int], int]

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

hget(

name: str,

key: str

) â 
Union[Awaitable[Optional[str]], Optional[str]]

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

hgetall(

name: str

) â 
Union[Awaitable[dict], dict]

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

```node.js
const res1 = await client.hSet(
  'bike:1',
  {
    'model': 'Deimos',
    'brand': 'Ergonom',
    'type': 'Enduro bikes',
    'price': 4972,
  }
)
console.log(res1) // 4

const res2 = await client.hGet('bike:1', 'model')
console.log(res2)  // 'Deimos'

const res3 = await client.hGet('bike:1', 'price')
console.log(res3)  // '4972'

const res4 = await client.hGetAll('bike:1')
console.log(res4)  
/*
{
  brand: 'Ergonom',
  model: 'Deimos',
  price: '4972',
  type: 'Enduro bikes'
}
*/
```

```node.js

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

HSET(

...[key, value, fieldValue]: SingleFieldArguments | MultipleFieldsArguments

) â 
Any

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

HGET(

key: RedisArgument,

field: RedisArgument

) â 
Any

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

HGETALL(

key: RedisArgument

) â 
Any

[Node.js Quick-Start](https://redis.io/docs/latest/develop/clients/nodejs/)

```java
Map<String, String> bike1 = new HashMap<>();
      bike1.put("model", "Deimos");
      bike1.put("brand", "Ergonom");
      bike1.put("type", "Enduro bikes");
      bike1.put("price", "4972");

      Long res1 = jedis.hset("bike:1", bike1);
      System.out.println(res1); // 4

      String res2 = jedis.hget("bike:1", "model");
      System.out.println(res2); // Deimos

      String res3 = jedis.hget("bike:1", "price");
      System.out.println(res3); // 4972

      Map<String, String> res4 = jedis.hgetAll("bike:1");
      System.out.println(res4); // {type=Enduro bikes, brand=Ergonom, price=4972, model=Deimos}
```

```java

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

hset(

key: byte[],

field: byte[],

value: byte[]

) â 
long // If the field already exists, and the HSET just produced an update of the value, 0 is returned, otherwise if a new field is created 1 is returned.

hset(

key: byte[],

hash: Map<byte[], byte[]>

) â 
long // If the field already exists, and the HSET just produced an update of the value, 0 is returned, otherwise if a new field is created 1 is returned.

hset(

key: String,

field: String,

value: String

) â 
long // If the field already exists, and the HSET just produced an update of the value, 0 is returned, otherwise if a new field is created 1 is returned.

hset(

key: String,

hash: Map<String, String>

) â 
long // If the field already exists, and the HSET just produced an update of the value, 0 is returned, otherwise if a new field is created 1 is returned.

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

hget(

key: String,

field: String

) â 
String // Bulk reply

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

hgetAll(

key: String

) â 
Map<String, String> // All the fields and values contained into a hash.

[Java-Sync Quick-Start](https://redis.io/docs/latest/develop/clients/jedis/)

```java
Map<String, String> bike1 = new HashMap<>();
            bike1.put("model", "Deimos");
            bike1.put("brand", "Ergonom");
            bike1.put("type", "Enduro bikes");
            bike1.put("price", "4972");

            CompletableFuture<Void> setGetAll = asyncCommands.hset("bike:1", bike1).thenCompose(res1 -> {
                System.out.println(res1); // >>> 4
                return asyncCommands.hget("bike:1", "model");
            }).thenCompose(res2 -> {
                System.out.println(res2); // >>> Deimos
                return asyncCommands.hget("bike:1", "price");
            }).thenCompose(res3 -> {
                System.out.println(res3); // >>> 4972
                return asyncCommands.hgetall("bike:1");
            })
                    .thenAccept(System.out::println)
                    // >>> {type=Enduro bikes, brand=Ergonom, price=4972, model=Deimos}
                    .toCompletableFuture();
```

```java

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

hset(

key: K, // the key of the hash.

field: K,

value: V

) â 
RedisFuture<Boolean> // Long integer-reply: the number of fields that were added. @since 5.3

hset(

key: K, // the key of the hash.

map: Map<K, V> // the field/value pairs to update.

) â 
RedisFuture<Long> // Long integer-reply: the number of fields that were added. @since 5.3

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

hget(

key: K, // the key.

field: K // the field type: key.

) â 
RedisFuture<V> // V bulk-string-reply the value associated with field, or null when field is not present in the hash or key does not exist.

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

hgetall(

key: K // the key.

) â 
RedisFuture<Map<K, V>> // Long count of the keys.

hgetall(

channel: KeyValueStreamingChannel<K, V>, // the channel.

key: K // the key.

) â 
RedisFuture<Long> // Long count of the keys.

[Java-Async Quick-Start](https://redis.io/docs/latest/develop/clients/lettuce/)

```java
Map<String, String> bike1 = new HashMap<>();
            bike1.put("model", "Deimos");
            bike1.put("brand", "Ergonom");
            bike1.put("type", "Enduro bikes");
            bike1.put("price", "4972");

            Mono<Long> setGetAll = reactiveCommands.hset("bike:1", bike1).doOnNext(result -> {
                System.out.println(result); // >>> 4
            });

            setGetAll.block();

            Mono<String> getModel = reactiveCommands.hget("bike:1", "model").doOnNext(result -> {
                System.out.println(result); // >>> Deimos
            });

            Mono<String> getPrice = reactiveCommands.hget("bike:1", "price").doOnNext(result -> {
                System.out.println(result); // >>> 4972
            });

            Mono<List<KeyValue<String, String>>> getAll = reactiveCommands.hgetall("bike:1").collectList().doOnNext(result -> {
                System.out.println(result);
                // >>> [KeyValue[type, Enduro bikes], KeyValue[brand, Ergonom],
                // KeyValue[price, 4972], KeyValue[model, Deimos]]
            });
```

```java

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

hset(

key: K, // the key of the hash.

field: K,

value: V

) â 
Mono<Boolean> // Long integer-reply: the number of fields that were added. @since 5.3

hset(

key: K, // the key of the hash.

map: Map<K, V> // the field/value pairs to update.

) â 
Mono<Long> // Long integer-reply: the number of fields that were added. @since 5.3

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

hget(

key: K, // the key.

field: K // the field type: key.

) â 
Mono<V> // V bulk-string-reply the value associated with field, or null when field is not present in the hash or key does not exist.

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

hgetall(

key: K // the key.

) â 
Flux<KeyValue<K, V>> // Long count of the keys. @deprecated since 6.0 in favor of consuming large results through the org.reactivestreams.Publisher returned by #hgetall.

hgetall(

channel: KeyValueStreamingChannel<K, V>, // the channel.

key: K // the key.

) â 
Mono<Long> // Long count of the keys. @deprecated since 6.0 in favor of consuming large results through the org.reactivestreams.Publisher returned by #hgetall.

[Java-Reactive Quick-Start](https://redis.io/docs/latest/develop/clients/lettuce/)

```go
hashFields := []string{
		"model", "Deimos",
		"brand", "Ergonom",
		"type", "Enduro bikes",
		"price", "4972",
	}

	res1, err := rdb.HSet(ctx, "bike:1", hashFields).Result()

	if err != nil {
		panic(err)
	}

	fmt.Println(res1) // >>> 4

	res2, err := rdb.HGet(ctx, "bike:1", "model").Result()

	if err != nil {
		panic(err)
	}

	fmt.Println(res2) // >>> Deimos

	res3, err := rdb.HGet(ctx, "bike:1", "price").Result()

	if err != nil {
		panic(err)
	}

	fmt.Println(res3) // >>> 4972

	cmdReturn := rdb.HGetAll(ctx, "bike:1")
	res4, err := cmdReturn.Result()

	if err != nil {
		panic(err)
	}

	fmt.Println(res4)
	// >>> map[brand:Ergonom model:Deimos price:4972 type:Enduro bikes]

	type BikeInfo struct {
		Model string `redis:"model"`
		Brand string `redis:"brand"`
		Type  string `redis:"type"`
		Price int    `redis:"price"`
	}

	var res4a BikeInfo

	if err := cmdReturn.Scan(&res4a); err != nil {
		panic(err)
	}

	fmt.Printf("Model: %v, Brand: %v, Type: %v, Price: $%v\n",
		res4a.Model, res4a.Brand, res4a.Type, res4a.Price)
	// >>> Model: Deimos, Brand: Ergonom, Type: Enduro bikes, Price: $4972
```

```go

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

HSet(

ctx: context.Context,

key: string,

values: ...interface{}

) â 
*IntCmd

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

HGet(

ctx: context.Context,

key: Any,

field: string

) â 
*StringCmd

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

HGetAll(

ctx: context.Context,

key: string

) â 
*MapStringStringCmd

[Go Quick-Start](https://redis.io/docs/latest/develop/clients/go/)

```c#
db.HashSet("bike:1", [
            new("model", "Deimos"),
            new("brand", "Ergonom"),
            new("type", "Enduro bikes"),
            new("price", 4972)
        ]);

        Console.WriteLine("Hash Created");
        // Hash Created

        var model = db.HashGet("bike:1", "model");
        Console.WriteLine($"Model: {model}");
        // Model: Deimos

        var price = db.HashGet("bike:1", "price");
        Console.WriteLine($"Price: {price}");
        // Price: 4972

        var bike = db.HashGetAll("bike:1");
        Console.WriteLine("bike:1");
        Console.WriteLine(string.Join("\n", bike.Select(b => $"{b.Name}: {b.Value}")));
        // Bike:1:
        // model: Deimos
        // brand: Ergonom
        // type: Enduro bikes
        // price: 4972
```

```c#

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

HashSet(

key: RedisKey, // The key of the hash.

hashFields: HashEntry[],

flags: CommandFlags // The flags to use for this operation.

) â 
void // true if field is a new field in the hash and value was set, false if field already exists in the hash and the value was updated.

HashSet(

key: RedisKey, // The key of the hash.

hashField: RedisValue, // The field to set in the hash.

value: RedisValue, // The value to set.

when: When, // Which conditions under which to set the field value (defaults to always).

flags: CommandFlags // The flags to use for this operation.

) â 
bool // true if field is a new field in the hash and value was set, false if field already exists in the hash and the value was updated.

HashSet(

key: RedisKey, // The key of the hash.

hashField: RedisValue, // The field to set in the hash.

value: RedisValue, // The value to set.

when: When, // Which conditions under which to set the field value (defaults to always).

flags: CommandFlags // The flags to use for this operation.

) â 
bool // true if field is a new field in the hash and value was set, false if field already exists in the hash and the value was updated.

HashSet(

key: RedisKey, // The key of the hash.

hashFields: HashEntry[],

flags: CommandFlags // The flags to use for this operation.

) â 
void // true if field is a new field in the hash and value was set, false if field already exists in the hash and the value was updated.

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

HashGet(

key: RedisKey, // The key of the hash.

hashField: RedisValue,

flags: CommandFlags // The flags to use for this operation.

) â 
RedisValue // List of values associated with the given fields, in the same order as they are requested.

HashGet(

key: RedisKey, // The key of the hash.

hashFields: RedisValue[], // The fields in the hash to get.

flags: CommandFlags // The flags to use for this operation.

) â 
RedisValue[] // List of values associated with the given fields, in the same order as they are requested.

HashGet(

key: RedisKey, // The key of the hash.

hashField: RedisValue,

flags: CommandFlags // The flags to use for this operation.

) â 
RedisValue // List of values associated with the given fields, in the same order as they are requested.

HashGet(

key: RedisKey, // The key of the hash.

hashFields: RedisValue[], // The fields in the hash to get.

flags: CommandFlags // The flags to use for this operation.

) â 
RedisValue[] // List of values associated with the given fields, in the same order as they are requested.

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

HashGetAll(

key: RedisKey, // The key of the hash to get all entries from.

flags: CommandFlags // The flags to use for this operation.

) â 
HashEntry[] // List of fields and their values stored in the hash, or an empty list when key does not exist.

HashGetAll(

key: RedisKey, // The key of the hash to get all entries from.

flags: CommandFlags // The flags to use for this operation.

) â 
HashEntry[] // List of fields and their values stored in the hash, or an empty list when key does not exist.

[C#-Sync (SE.Redis) Quick-Start](https://redis.io/docs/latest/develop/clients/dotnet/)

```php
$res1 = $r->hmset('bike:1', [
            'model' => 'Deimos',
            'brand' => 'Ergonom',
            'type' => 'Enduro bikes',
            'price' => 4972,
        ]);

        echo $res1 . PHP_EOL;
        // >>> 4

        $res2 = $r->hget('bike:1', 'model');
        echo $res2 . PHP_EOL;
        // >>> Deimos

        $res3 = $r->hget('bike:1', 'price');
        echo $res3 . PHP_EOL;
        // >>> 4972

        $res4 = $r->hgetall('bike:1');
        echo json_encode($res3) . PHP_EOL;
        // >>> {"name":"Deimos","brand":"Ergonom","type":"Enduro bikes","price":"4972"}
```

```php

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

hset(

$key: string,

$field: string,

$value: string

) â 
int

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

hget(

$key: string,

$field: string

) â 
string|null

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

hgetall(

$key: string

) â 
array

[PHP Quick-Start](https://redis.io/docs/latest/develop/clients/php/)

```ruby
res1 = r.hset('bike:1', {
  'model' => 'Deimos',
  'brand' => 'Ergonom',
  'type' => 'Enduro bikes',
  'price' => 4972
})
puts res1 # 4

res2 = r.hget('bike:1', 'model')
puts res2 # Deimos

res3 = r.hget('bike:1', 'price')
puts res3 # 4972

res4 = r.hgetall('bike:1')
puts res4.inspect
# {"model"=>"Deimos", "brand"=>"Ergonom", "type"=>"Enduro bikes", "price"=>"4972"}
```

```ruby

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

hset(

key: String,

*attrs: Array<String> | Hash<String, String> // array or hash of fields and values

) â 
Integer // The number of fields that were added to the hash

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

hget(

key: String,

field: String

) â 
String

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

hgetall(

key: String

) â 
Hash<String, String>

[Ruby Quick-Start](https://redis.io/docs/latest/develop/clients/redis-rb/)

```rust
let hash_fields = [
            ("model", "Deimos"),
            ("brand", "Ergonom"),
            ("type", "Enduro bikes"),
            ("price", "4972"),
        ];

        if let Ok(res) = r.hset_multiple("bike:1", &hash_fields) {
            let res: String = res;
            println!("{res}");    // >>> OK
        }

        match r.hget("bike:1", "model") {
            Ok(res) => {
                let res: String = res;
                println!("{res}");   // >>> Deimos
            },
            Err(e) => {
                println!("Error getting bike:1 model: {e}");
                return;
            }
        };

        match r.hget("bike:1", "price") {
            Ok(res) => {
                let res: String = res;
                println!("{res}");   // >>> 4972
            },
            Err(e) => {
                println!("Error getting bike:1 price: {e}");
                return;
            }
        };

        match r.hgetall("bike:1") {
            Ok(res) => {
                let res: Vec<(String, String)> = res;
                println!("{res:?}");
                // >>> [("model", "Deimos"), ("brand", "Ergonom"), ("type", "Enduro bikes"), ("price", "4972")]
            },
            Err(e) => {
                println!("Error getting bike:1: {e}");
                return;
            }
        };
```

```rust

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

hset(

key: K,

field: F,

value: V

) â 
(usize)

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

hget(

key: K,

field: F

) â 
(Option<String>)

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

hgetall(

key: K

) â 
(std::collections::HashMap<String, String>)

[Rust-Sync Quick-Start](https://redis.io/docs/latest/develop/clients/rust/)

```rust
let hash_fields = [
            ("model", "Deimos"),
            ("brand", "Ergonom"),
            ("type", "Enduro bikes"),
            ("price", "4972"),
        ];

        if let Ok(res) = r.hset_multiple("bike:1", &hash_fields).await {
            let res: String = res;
            println!("{res}");    // >>> OK
        }

        match r.hget("bike:1", "model").await {
            Ok(res) => {
                let res: String = res;
                println!("{res}");   // >>> Deimos
            },
            Err(e) => {
                println!("Error getting bike:1 model: {e}");
                return;
            }
        };

        match r.hget("bike:1", "price").await {
            Ok(res) => {
                let res: String = res;
                println!("{res}");   // >>> 4972
            },
            Err(e) => {
                println!("Error getting bike:1 price: {e}");
                return;
            }
        };

        match r.hgetall("bike:1").await {
            Ok(res) => {
                let res: Vec<(String, String)> = res;
                println!("{res:?}");
                // >>> [("model", "Deimos"), ("brand", "Ergonom"), ("type", "Enduro bikes"), ("price", "4972")]
            },
            Err(e) => {
                println!("Error getting bike:1: {e}");
                return;
            }
        };
```

```rust

```

[HSET](https://redis.io/docs/latest/commands/hset/)

                                    (
                                    
                                        
                                        
                                        [@write](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#write)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Creates or modifies the value of a field in a hash.

hset(

key: K,

field: F,

value: V

) â 
(usize)

[HGET](https://redis.io/docs/latest/commands/hget/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@fast](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#fast)
                                    
                                    )
                                

Returns the value of a field in a hash.

hget(

key: K,

field: F

) â 
(Option<String>)

[HGETALL](https://redis.io/docs/latest/commands/hgetall/)

                                    (
                                    
                                        
                                        
                                        [@read](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#read)
, 
[@hash](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#hash)
, 
[@slow](https://redis.io/docs/latest/operate/oss_and_stack/management/security/acl/#slow)
                                    
                                    )
                                

Returns all fields and values in a hash.

hgetall(

key: K

) â 
(std::collections::HashMap<String, String>)

[Rust-Async Quick-Start](https://redis.io/docs/latest/develop/clients/rust/)

You can get a complete overview of available data types in this documentation site's [data types section](/docs/latest/develop/data-types/). Each data type has commands allowing you to manipulate or retrieve data. The [[commands|commands reference]] provides a sophisticated explanation.

## Scan the keyspace

Each item within Redis has a unique key. All items live within the Redis [[develop/using-commands/keyspace|keyspace]]. You can scan the Redis keyspace via the [SCAN command](/docs/latest/commands/scan/). Here is an example that scans for the first 100 keys that have the prefix `bike:`:

```
SCAN 0 MATCH "bike:*" COUNT 100
```

[SCAN](/docs/latest/commands/scan/) returns a cursor position, allowing you to scan iteratively for the next batch of keys until you reach the cursor value 0.

## Next steps

You can address more use cases with Redis by reading these additional quick start guides:

[[develop/get-started/document-database|Redis as a document database]]
[[develop/get-started/vector-database|Redis as a vector database]]

## Continue learning with Redis University

See the [Get Started with Redis learning path](https://university.redis.io/learningpath/14q8m6gilfwltm) for courses.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page