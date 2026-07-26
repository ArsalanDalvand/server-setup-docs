---
title: "Redis as a document database quick start guide"
source: "https://redis.io/docs/latest/develop/get-started/document-database/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/get-started/document-database"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis as a document database quick start guide"
---
# Redis as a document database quick start guide

Understand how to use Redis as a document database

This quick start guide shows you how to:

Create a secondary index
Add [JSON](/docs/latest/develop/data-types/json/) documents
Search and query your data

The examples in this article refer to a simple bicycle inventory that contains JSON documents with the following structure:

```json
{
  "brand": "brand name",
  "condition": "new | used | refurbished",
  "description": "description",
  "model": "model",
  "price": 0
}
```

## Setup

The easiest way to get started with [[operate/oss_and_stack|Redis]] is to use Redis Cloud:

Create a [free account](https://redis.com/try-free?utm_source=redisio&utm_medium=referral&utm_campaign=2023-09-try_free&utm_content=cu-redis_cloud_users).

Follow the instructions to create a free database.

This free Redis Cloud database comes out of the box with all the Redis Open Source features.

You can alternatively use the [[operate/oss_and_stack/install/install-stack|installation guides]] to install Redis Open Source on your local machine.

## Connect

The first step is to connect to your Redis Open Source database. You can find further details about the connection options in this documentation site's [[develop/tools|Tools section]]. The following example shows how to connect to a Redis Open Source server that runs on localhost (`-h 127.0.0.1`) and listens on the default port (`-p 6379`):

Language:

                        >_ Redis CLI
                    

                        Python
                    

                        JavaScript (node-redis)
                    

                        Java-Sync
                    

                        Go
                    

                        C#-Sync (NRedisStack)
                    

Foundational: Connect to a Redis server using redis-cli with host and port parameters

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
  You can copy and paste the connection details from the Redis Cloud database configuration page. Here is an example connection string of a Cloud database that is hosted in the AWS region `us-east-1` and listens on port 16379: `redis-16379.c283.us-east-1-4.ec2.cloud.redislabs.com:16379`. The connection string has the format `host:port`. You must also copy and paste your Cloud database's username and password and then pass the credentials to your client or use the [AUTH command](/docs/latest/commands/auth/) after the connection is established.

## Create an index and add documents

As explained in the [[develop/get-started/data-store|in-memory data store]] quick start guide, Redis allows you to access an item directly via its key. You also learned how to scan the keyspace. Whereby you can use other data structures (e.g., hashes and sorted sets) as secondary indexes, your application would need to maintain those indexes manually. Redis is a document database that allows you to declare which fields are auto-indexed. Redis currently supports secondary index creation on the [hashes](/docs/latest/develop/data-types/hashes/) and [JSON](/docs/latest/develop/data-types/json/) documents.

The following example uses an [FT.CREATE](/docs/latest/commands/ft.create/) command to create an index with some text fields, a numeric field (price), and a tag field (condition). The text fields have a weight of 1.0, meaning they have the same relevancy in the context of full-text searches. The field names follow the [JSONPath](/docs/latest/develop/data-types/json/path/) notion. Each such index field maps to a property within the JSON document. The example then uses the [JSON.SET](/docs/latest/commands/json.set/) command to add the JSON documents that the index tracks.

Any pre-existing JSON documents with a key prefix `bicycle:` are automatically added to the index. Additionally, any JSON documents with that prefix created or modified after index creation are added or re-added to the index.

Language:

                        >_ Redis CLI
                    

                        Python
                    

                        JavaScript (node-redis)
                    

                        Java-Sync
                    

                        Go
                    

                        C#-Sync (NRedisStack)
                    

Foundational: Create an index on JSON documents with FT.CREATE, then add the documents with JSON.SET

FT.CREATE idx:bicycle ON JSON PREFIX 1 bicycle: SCORE 1.0 SCHEMA $.brand AS brand TEXT WEIGHT 1.0 $.model AS model TEXT WEIGHT 1.0 $.description AS description TEXT WEIGHT 1.0 $.price AS price NUMERIC $.condition AS condition TAG SEPARATOR ,
JSON.SET "bicycle:0" "." "{\"brand\": \"Velorim\", \"model\": \"Jigger\", \"price\": 270, \"description\": \"Small and powerful, the Jigger is the best ride for the smallest of tikes! This is the tiniest kids\\u2019 pedal bike on the market available without a coaster brake, the Jigger is the vehicle of choice for the rare tenacious little rider raring to go.\", \"condition\": \"new\"}"
JSON.SET "bicycle:1" "." "{\"brand\": \"Bicyk\", \"model\": \"Hillcraft\", \"price\": 1200, \"description\": \"Kids want to ride with as little weight as possible. Especially on an incline! They may be at the age when a 27.5\\\" wheel bike is just too clumsy coming off a 24\\\" bike. The Hillcraft 26 is just the solution they need!\", \"condition\": \"used\"}"
JSON.SET "bicycle:2" "." "{\"brand\": \"Nord\", \"model\": \"Chook air 5\", \"price\": 815, \"description\": \"The Chook Air 5  gives kids aged six years and older a durable and uberlight mountain bike for their first experience on tracks and easy cruising through forests and fields. The lower  top tube makes it easy to mount and dismount in any situation, giving your kids greater safety on the trails.\", \"condition\": \"used\"}"
JSON.SET "bicycle:3" "." "{\"brand\": \"Eva\", \"model\": \"Eva 291\", \"price\": 3400, \"description\": \"The sister company to Nord, Eva launched in 2005 as the first and only women-dedicated bicycle brand. Designed by women for women, allEva bikes are optimized for the feminine physique using analytics from a body metrics database. If you like 29ers, try the Eva 291. It\\u2019s a brand new bike for 2022.. This full-suspension, cross-country ride has been designed for velocity. The 291 has 100mm of front and rear travel, a superlight aluminum frame and fast-rolling 29-inch wheels. Yippee!\", \"condition\": \"used\"}"
JSON.SET "bicycle:4" "." "{\"brand\": \"Noka Bikes\", \"model\": \"Kahuna\", \"price\": 3200, \"description\": \"Whether you want to try your hand at XC racing or are looking for a lively trail bike that's just as inspiring on the climbs as it is over rougher ground, the Wilder is one heck of a bike built specifically for short women. Both the frames and components have been tweaked to include a women\\u2019s saddle, different bars and unique colourway.\", \"condition\": \"used\"}"
JSON.SET "bicycle:5" "." "{\"brand\": \"Breakout\", \"model\": \"XBN 2.1 Alloy\", \"price\": 810, \"description\": \"The XBN 2.1 Alloy is our entry-level road bike \\u2013 but that\\u2019s not to say that it\\u2019s a basic machine. With an internal weld aluminium frame, a full carbon fork, and the slick-shifting Claris gears from Shimano\\u2019s, this is a bike which doesn\\u2019t break the bank and delivers craved performance.\", \"condition\": \"new\"}"
JSON.SET "bicycle:6" "." "{\"brand\": \"ScramBikes\", \"model\": \"WattBike\", \"price\": 2300, \"description\": \"An e-bike with a 1000W mid-drive and a 48V battery, offering over 60 miles per charge and three riding modes.\", \"condition\": \"new\"}"
JSON.SET "bicycle:7" "." "{\"brand\": \"Peaknetic\", \"model\": \"Secto\", \"price\": 430, \"description\": \"A lightweight aluminum commuter bike with ergonomic grips, a lumbar-supporting seat, and a low step-over frame for comfort and easy mounting.\", \"condition\": \"new\"}"
JSON.SET "bicycle:8" "." "{\"brand\": \"nHill\", \"model\": \"Summit\", \"price\": 1200, \"description\": \"This budget mountain bike from nHill performs well both on bike paths and on the trail. The fork with 100mm of travel absorbs rough terrain. Fat Kenda Booster tires give you grip in corners and on wet trails. The Shimano Tourney drivetrain offered enough gears for finding a comfortable pace to ride uphill, and the Tektro hydraulic disc brakes break smoothly. Whether you want an affordable bike that you can take to work, but also take trail in mountains on the weekends or you\\u2019re just after a stable, comfortable ride for the bike path, the Summit gives a good value for money.\", \"condition\": \"new\"}"
JSON.SET "bicycle:9" "." "{\"model\": \"ThrillCycle\", \"brand\": \"BikeShind\", \"price\": 815, \"description\": \"An artsy,  retro-inspired bicycle that\\u2019s as functional as it is pretty: The ThrillCycle steel frame offers a smooth ride. A 9-speed drivetrain has enough gears for coasting in the city, but we wouldn\\u2019t suggest taking it to the mountains. Fenders protect you from mud, and a rear basket lets you transport groceries, flowers and books. The ThrillCycle comes with a limited lifetime warranty, so this little guy will last you long past graduation.\", \"condition\": \"refurbished\"}"

[FT.CREATE](https://redis.io/docs/latest/commands/ft.create/)

Creates an index with the given spec

[JSON.SET](https://redis.io/docs/latest/commands/json.set/)

Sets or updates the JSON value at a path

[Redis CLI guide](https://redis.io/docs/latest/develop/tools/cli/)

                            Also, check out our other client tools
                            [Redis Insight](https://redis.io/docs/latest/develop/tools/insight/)
                            and
                            [Redis for VS Code](https://redis.io/docs/latest/develop/tools/redis-for-vscode/).
                        

```python
schema = (
    TextField("$.brand", as_name="brand"),
    TextField("$.model", as_name="model"),
    TextField("$.description", as_name="description"),
    NumericField("$.price", as_name="price"),
    TagField("$.condition", as_name="condition"),
)

index = r.ft("idx:bicycle")
index.create_index(
    schema,
    definition=IndexDefinition(prefix=["bicycle:"], index_type=IndexType.JSON),
)
for bid, bicycle in enumerate(bicycles):
    r.json().set(f"bicycle:{bid}", Path.root_path(), bicycle)
```

[FT.CREATE](https://redis.io/docs/latest/commands/ft.create/)

Creates an index with the given spec

create_index(

fields: Iterable[Field], // The fields to index

definition: Optional[IndexDefinition] // Optional index definition

) â 
str // OK on success

[JSON.SET](https://redis.io/docs/latest/commands/json.set/)

Sets or updates the JSON value at a path

set(

name: str,

path: str,

obj: JsonType,

nx: Optional[bool] = False,

xx: Optional[bool] = False,

decode_keys: Optional[bool] = False

) â 
Optional[str]

jsonset(

*args: Any,

**kwargs: Any

) â 
Any

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

```node.js
const schema = {
  '$.brand': {
    type: SCHEMA_FIELD_TYPE.TEXT,
    SORTABLE: true,
    AS: 'brand'
  },
  '$.model': {
    type: SCHEMA_FIELD_TYPE.TEXT,
    AS: 'model'
  },
  '$.description': {
    type: SCHEMA_FIELD_TYPE.TEXT,
    AS: 'description'
  },
  '$.price': {
    type: SCHEMA_FIELD_TYPE.NUMERIC,
    AS: 'price'
  },
  '$.condition': {
    type: SCHEMA_FIELD_TYPE.TAG,
    AS: 'condition'
  }
};

try {
  await client.ft.create('idx:bicycle', schema, {
    ON: 'JSON',
    PREFIX: 'bicycle:'
  });
} catch (e) {
  if (e.message === 'Index already exists') {
    console.log('Index exists already, skipped creation.');
  } else {
    // Something went wrong, perhaps RediSearch isn't installed...
    console.error(e);
    process.exit(1);
  }
}

await Promise.all(
  bicycles.map((bicycle, i) => client.json.set(`bicycle:${i}`, '$', bicycle))
);
```

[FT.CREATE](https://redis.io/docs/latest/commands/ft.create/)

Creates an index with the given spec

CREATE(

index: RedisArgument, // The index name

schema: Array<SchemaField>, // The schema fields

options: FTCreateOptions // Optional create options

) â 
Promise<string> // OK on success

[JSON.SET](https://redis.io/docs/latest/commands/json.set/)

Sets or updates the JSON value at a path

SET(

key: RedisArgument,

path: RedisArgument,

json: RedisJSON,

options?: JsonSetOptions

) â 
Any

[Node.js Quick-Start](https://redis.io/docs/latest/develop/clients/nodejs/)

```java
SchemaField[] schema = {
      TextField.of("$.brand").as("brand"),
      TextField.of("$.model").as("model"),
      TextField.of("$.description").as("description"),
      NumericField.of("$.price").as("price"),
      TagField.of("$.condition").as("condition")
    };

    jedis.ftCreate("idx:bicycle",
      FTCreateParams.createParams()
      .on(IndexDataType.JSON)
      .addPrefix("bicycle:"),
      schema
    );

    Bicycle[] bicycles = {
        new Bicycle(
            "Velorim",
            "Jigger",
            new BigDecimal(270),
            "new",
            "Small and powerful, the Jigger is the best ride " +
            "for the smallest of tikes! This is the tiniest " +
            "kidsâ pedal bike on the market available without" +
            " a coaster brake, the Jigger is the vehicle of " +
            "choice for the rare tenacious little rider " +
            "raring to go."
        ),
        new Bicycle(
            "Bicyk",
            "Hillcraft",
            new BigDecimal(1200),
            "used",
            "Kids want to ride with as little weight as possible." +
            " Especially on an incline! They may be at the age " +
            "when a 27.5 inch wheel bike is just too clumsy coming " +
            "off a 24 inch bike. The Hillcraft 26 is just the solution" +
            " they need!"
        ),
        new Bicycle(
            "Nord",
            "Chook air 5",
            new BigDecimal(815),
            "used",
            "The Chook Air 5  gives kids aged six years and older " +
            "a durable and uberlight mountain bike for their first" +
            " experience on tracks and easy cruising through forests" +
            " and fields. The lower  top tube makes it easy to mount" +
            " and dismount in any situation, giving your kids greater" +
            " safety on the trails."
        ),
        new Bicycle(
            "Eva",
            "Eva 291",
            new BigDecimal(3400),
            "used",
            "The sister company to Nord, Eva launched in 2005 as the" +
            " first and only women-dedicated bicycle brand. Designed" +
            " by women for women, allEva bikes are optimized for the" +
            " feminine physique using analytics from a body metrics" +
            " database. If you like 29ers, try the Eva 291. It's a " +
            "brand new bike for 2022.. This full-suspension, " +
            "cross-country ride has been designed for velocity. The" +
            " 291 has 100mm of front and rear travel, a superlight " +
            "aluminum frame and fast-rolling 29-inch wheels. Yippee!"
        ),
        new Bicycle(
            "Noka Bikes",
            "Kahuna",
            new BigDecimal(3200),
            "used",
            "Whether you want to try your hand at XC racing or are " +
            "looking for a lively trail bike that's just as inspiring" +
            " on the climbs as it is over rougher ground, the Wilder" +
            " is one heck of a bike built specifically for short women." +
            " Both the frames and components have been tweaked to " +
            "include a womenâs saddle, different bars and unique " +
            "colourway."
        ),
        new Bicycle(
            "Breakout",
            "XBN 2.1 Alloy",
            new BigDecimal(810),
            "new",
            "The XBN 2.1 Alloy is our entry-level road bike â but thatâs" +
            " not to say that itâs a basic machine. With an internal " +
            "weld aluminium frame, a full carbon fork, and the slick-shifting" +
            " Claris gears from Shimanoâs, this is a bike which doesnât" +
            " break the bank and delivers craved performance."
        ),
        new Bicycle(
            "ScramBikes",
            "WattBike",
            new BigDecimal(2300),
            "new",
            "An e-bike with a 1000W mid-drive and a 48V battery, offering over 60 miles per charge and three riding modes."
        ),
        new Bicycle(
            "Peaknetic",
            "Secto",
            new BigDecimal(430),
            "new",
            "A lightweight aluminum commuter bike with ergonomic grips, a lumbar-supporting seat, and a low step-over frame for comfort and easy mounting."
        ),
        new Bicycle(
            "nHill",
            "Summit",
            new BigDecimal(1200),
            "new",
            "This budget mountain bike from nHill performs well both on bike" +
            " paths and on the trail. The fork with 100mm of travel absorbs" +
            " rough terrain. Fat Kenda Booster tires give you grip in corners" +
            " and on wet trails. The Shimano Tourney drivetrain offered enough" +
            " gears for finding a comfortable pace to ride uphill, and the" +
            " Tektro hydraulic disc brakes break smoothly. Whether you want an" +
            " affordable bike that you can take to work, but also take trail in" +
            " mountains on the weekends or youâre just after a stable," +
            " comfortable ride for the bike path, the Summit gives a good value" +
            " for money."
        ),
        new Bicycle(
            "BikeShind",
            "ThrillCycle",
            new BigDecimal(815),
            "refurbished",
            "An artsy,  retro-inspired bicycle thatâs as functional as it is" +
            " pretty: The ThrillCycle steel frame offers a smooth ride. A" +
            " 9-speed drivetrain has enough gears for coasting in the city, but" +
            " we wouldnât suggest taking it to the mountains. Fenders protect" +
            " you from mud, and a rear basket lets you transport groceries," +
            " flowers and books. The ThrillCycle comes with a limited lifetime" +
            " warranty, so this little guy will last you long past graduation."
        ),
    };

    for (int i = 0; i < bicycles.length; i++) {
      jedis.jsonSetWithEscape(String.format("bicycle:%d", i), bicycles[i]);
    }
```

[FT.CREATE](https://redis.io/docs/latest/commands/ft.create/)

Creates an index with the given spec

ftCreate(

indexName: String, // The index name

indexOptions: IndexOptions, // Index options

schema: Schema // The schema definition

) â 
String // OK on success

ftCreate(

indexName: String, // The index name

createParams: FTCreateParams, // Create parameters

schemaFields: Iterable<SchemaField> // The schema fields

) â 
String // OK on success

[JSON.SET](https://redis.io/docs/latest/commands/json.set/)

Sets or updates the JSON value at a path

jsonSet(

key: Any,

Path.ROOT_PATH: Any,

pojo: Any

) â 
return

jsonSet(

key: Any,

Path.ROOT_PATH: Any,

pojo: Any,

params: Any

) â 
return

jsonSet(

key: String,

path: Path,

pojo: Object

) â 
String

jsonSet(

key: String,

path: Path,

pojo: Object,

params: JsonSetParams

) â 
String

jsonSet(

key: String,

object: Object

) â 
default String

[Java-Sync Quick-Start](https://redis.io/docs/latest/develop/clients/jedis/)

```go
schema := []*redis.FieldSchema{
		{
			FieldName: "$.brand",
			As:        "brand",
			FieldType: redis.SearchFieldTypeText,
		},
		{
			FieldName: "$.model",
			As:        "model",
			FieldType: redis.SearchFieldTypeText,
		},
		{
			FieldName: "$.description",
			As:        "description",
			FieldType: redis.SearchFieldTypeText,
		},
		{
			FieldName: "$.price",
			As:        "price",
			FieldType: redis.SearchFieldTypeNumeric,
		},
		{
			FieldName: "$.condition",
			As:        "condition",
			FieldType: redis.SearchFieldTypeTag,
		},
	}

	_, err := rdb.FTCreate(ctx, "idx:bicycle",
		&redis.FTCreateOptions{
			Prefix: []interface{}{"bicycle:"},
			OnJSON: true,
		},
		schema...,
	).Result()

	if err != nil {
		panic(err)
	}

	for i, bicycle := range bicycles {
		_, err := rdb.JSONSet(
			ctx,
			fmt.Sprintf("bicycle:%v", i),
			"$",
			bicycle,
		).Result()

		if err != nil {
			panic(err)
		}
	}
```

[FT.CREATE](https://redis.io/docs/latest/commands/ft.create/)

Creates an index with the given spec

FTCreate(

ctx: context.Context,

index: string, // The index name

options: *FTCreateOptions, // Create options

schema: ...*FieldSchema // The schema fields

) â 
*StatusCmd // OK on success

[JSON.SET](https://redis.io/docs/latest/commands/json.set/)

Sets or updates the JSON value at a path

JSONSet(

ctx: context.Context,

key: Any,

path: string,

value: interface{}

) â 
*StatusCmd

[Go Quick-Start](https://redis.io/docs/latest/develop/clients/go/)

```c#
var schema = new Schema()
            .AddTextField(new FieldName("$.brand", "brand"))
            .AddTextField(new FieldName("$.model", "model"))
            .AddTextField(new FieldName("$.description", "description"))
            .AddNumericField(new FieldName("$.price", "price"))
            .AddTagField(new FieldName("$.condition", "condition"));

        ft.Create(
            "idx:bicycle",
            new FTCreateParams().On(IndexDataType.JSON).Prefix("bicycle:"),
            schema);

        for (int i = 0; i < bicycles.Length; i++)
        {
            json.Set($"bicycle:{i}", "$", bicycles[i]);
        }
```

[FT.CREATE](https://redis.io/docs/latest/commands/ft.create/)

Creates an index with the given spec

Create(

indexName: string, // The index name

parameters: FTCreateParams, // Index creation parameters

schema: Schema // The index schema

) â 
bool // true if the index was created

Create(

indexName: string, // The index name

schema: Schema // The index schema

) â 
bool // true if the index was created

[JSON.SET](https://redis.io/docs/latest/commands/json.set/)

Sets or updates the JSON value at a path

Set(

key: Any,

path: Any,

json: Any,

when: Any

) â 
return

Set(

key: RedisKey,

path: RedisValue,

json: RedisValue,

when: When

) â 
bool

Set(

key: Any,

path: Any,

fileContent: Any,

when: Any

) â 
return

[C#-Sync (NRedisStack) Quick-Start](https://redis.io/docs/latest/develop/clients/dotnet/)

## Search and query using Redis Search

### Wildcard query

You can retrieve all indexed documents using the [FT.SEARCH](/docs/latest/commands/ft.search/) command. Note the `LIMIT` clause below, which allows result pagination.

Language:

                        >_ Redis CLI
                    

                        Python
                    

                        JavaScript (node-redis)
                    

                        Java-Sync
                    

                        Go
                    

                        C#-Sync (NRedisStack)
                    

Foundational: Retrieve all indexed documents using FT.SEARCH with wildcard query

FT.SEARCH "idx:bicycle" "*" LIMIT 0 10

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

[Redis CLI guide](https://redis.io/docs/latest/develop/tools/cli/)

                            Also, check out our other client tools
                            [Redis Insight](https://redis.io/docs/latest/develop/tools/insight/)
                            and
                            [Redis for VS Code](https://redis.io/docs/latest/develop/tools/redis-for-vscode/).
                        

```python
res = index.search(Query("*"))
print("Documents found:", res.total)
# >>> Documents found: 10
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

search(

query: Union[str, Query], // The search query

query_params: Optional[Dict[str, Union[str, int, float]]] // Optional query parameters

) â 
Result // The search results

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

```node.js
let result = await client.ft.search('idx:bicycle', '*', {
  LIMIT: {
    from: 0,
    size: 10
  }
});

console.log(JSON.stringify(result, null, 2));

/*
{
  "total": 10,
  "documents": ...
}
*/
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

SEARCH(

index: RedisArgument, // The index name

query: RedisArgument, // The search query

options: FTSearchOptions // Optional search options

) â 
Promise<FTSearchResult> // The search results

[Node.js Quick-Start](https://redis.io/docs/latest/develop/clients/nodejs/)

```java
Query query1 = new Query("*");
    List<Document> result1 = jedis.ftSearch("idx:bicycle", query1).getDocuments();
    System.out.println("Documents found:" + result1.size());
    // Prints: Documents found: 10
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

ftSearch(

indexName: String, // The index name

query: Query // The search query

) â 
SearchResult // The search results

ftSearch(

indexName: String, // The index name

query: String, // The search query

searchParams: FTSearchParams // Search parameters

) â 
SearchResult // The search results

[Java-Sync Quick-Start](https://redis.io/docs/latest/develop/clients/jedis/)

```go
wCardResult, err := rdb.FTSearch(ctx, "idx:bicycle", "*").Result()

	if err != nil {
		panic(err)
	}

	fmt.Printf("Documents found: %v\n", wCardResult.Total)
	// >>> Documents found: 10
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

FTSearch(

ctx: context.Context,

index: string, // The index name

query: string // The search query

) â 
*FTSearchCmd // The search results

FTSearchWithArgs(

ctx: context.Context,

index: string, // The index name

query: string, // The search query

options: *FTSearchOptions // Search options

) â 
*FTSearchCmd // The search results

[Go Quick-Start](https://redis.io/docs/latest/develop/clients/go/)

```c#
var query1 = new Query("*");
        var res1 = ft.Search("idx:bicycle", query1).Documents;
        Console.WriteLine(string.Join("\n", res1.Count()));
        // Prints: Documents found: 10
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

Search(

indexName: string, // The index name

q: Query // The search query

) â 
SearchResult // Search results

[C#-Sync (NRedisStack) Quick-Start](https://redis.io/docs/latest/develop/clients/dotnet/)

### Single-term full-text query

The following command shows a simple single-term query for finding all bicycles with a specific model:

Language:

                        >_ Redis CLI
                    

                        Python
                    

                        JavaScript (node-redis)
                    

                        Java-Sync
                    

                        Go
                    

                        C#-Sync (NRedisStack)
                    

Foundational: Perform a single-term full-text query using FT.SEARCH to find documents matching a specific field value

FT.SEARCH "idx:bicycle" "@model:Jigger" LIMIT 0 10

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

[Redis CLI guide](https://redis.io/docs/latest/develop/tools/cli/)

                            Also, check out our other client tools
                            [Redis Insight](https://redis.io/docs/latest/develop/tools/insight/)
                            and
                            [Redis for VS Code](https://redis.io/docs/latest/develop/tools/redis-for-vscode/).
                        

```python
res = index.search(Query("@model:Jigger"))
print(res)
# >>> Result{1 total, docs: [
# Document {
#   'id': 'bicycle:0',
#   'payload': None,
#   'json': '{
#       "brand":"Velorim",
#       "model":"Jigger",
#       "price":270,
#       ...
#       "condition":"new"
#    }'
# }]}
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

search(

query: Union[str, Query], // The search query

query_params: Optional[Dict[str, Union[str, int, float]]] // Optional query parameters

) â 
Result // The search results

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

```node.js
result = await client.ft.search(
  'idx:bicycle',
  '@model:Jigger',
  {
    LIMIT: {
    from: 0,
    size: 10
  }
});

console.log(JSON.stringify(result, null, 2));
/*
{
  "total": 1,
  "documents": [{
    "id": "bicycle:0",
    "value": {
      "brand": "Velorim",
      "model": "Jigger",
      "price": 270,
      "description": "Small and powerful, the Jigger is the best ride for the smallest of tikes! This is the tiniest kidsâ pedal bike on the market available without a coaster brake, the Jigger is the vehicle of choice for the rare tenacious little rider raring to go.",
      "condition": "new"
    }
  }]
}
 */
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

SEARCH(

index: RedisArgument, // The index name

query: RedisArgument, // The search query

options: FTSearchOptions // Optional search options

) â 
Promise<FTSearchResult> // The search results

[Node.js Quick-Start](https://redis.io/docs/latest/develop/clients/nodejs/)

```java
Query query2 = new Query("@model:Jigger");
    List<Document> result2 = jedis.ftSearch("idx:bicycle", query2).getDocuments();
    System.out.println(result2);
    // Prints: [id:bicycle:0, score: 1.0, payload:null,
    // properties:[$={"brand":"Velorim","model":"Jigger","price":270,"description":"Small and powerful, the Jigger is the best ride for the smallest of tikes! This is the tiniest kidsâ pedal bike on the market available without a coaster brake, the Jigger is the vehicle of choice for the rare tenacious little rider raring to go.","condition":"new"}]]
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

ftSearch(

indexName: String, // The index name

query: Query // The search query

) â 
SearchResult // The search results

ftSearch(

indexName: String, // The index name

query: String, // The search query

searchParams: FTSearchParams // Search parameters

) â 
SearchResult // The search results

[Java-Sync Quick-Start](https://redis.io/docs/latest/develop/clients/jedis/)

```go
stResult, err := rdb.FTSearch(
		ctx,
		"idx:bicycle",
		"@model:Jigger",
	).Result()

	if err != nil {
		panic(err)
	}

	fmt.Println(stResult)
	// >>> {1 [{bicycle:0 <nil> <nil> <nil> map[$:{"brand":"Velorim", ...
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

FTSearch(

ctx: context.Context,

index: string, // The index name

query: string // The search query

) â 
*FTSearchCmd // The search results

FTSearchWithArgs(

ctx: context.Context,

index: string, // The index name

query: string, // The search query

options: *FTSearchOptions // Search options

) â 
*FTSearchCmd // The search results

[Go Quick-Start](https://redis.io/docs/latest/develop/clients/go/)

```c#
var query2 = new Query("@model:Jigger");
        var res2 = ft.Search("idx:bicycle", query2).Documents;
        Console.WriteLine(string.Join("\n", res2.Select(x => x["json"])));
        // Prints: {"brand":"Moore PLC","model":"Award Race","price":3790.76,
        //          "description":"This olive folding bike features a carbon frame
        //          and 27.5 inch wheels. This folding bike is perfect for compact
        //          storage and transportation.","condition":"new"}
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

Search(

indexName: string, // The index name

q: Query // The search query

) â 
SearchResult // Search results

[C#-Sync (NRedisStack) Quick-Start](https://redis.io/docs/latest/develop/clients/dotnet/)

### Exact match query

Below is a command to perform an exact match query that finds all bicycles with the brand name `Noka Bikes`. You must use double quotes around the search term when constructing an exact match query on a  text field.

Language:

                        >_ Redis CLI
                    

                        Python
                    

                        JavaScript (node-redis)
                    

                        Java-Sync
                    

                        Go
                    

                        C#-Sync (NRedisStack)
                    

Foundational: Perform an exact match query using FT.SEARCH with double quotes to find documents with precise field values

FT.SEARCH "idx:bicycle" "@brand:\"Noka Bikes\"" LIMIT 0 10

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

[Redis CLI guide](https://redis.io/docs/latest/develop/tools/cli/)

                            Also, check out our other client tools
                            [Redis Insight](https://redis.io/docs/latest/develop/tools/insight/)
                            and
                            [Redis for VS Code](https://redis.io/docs/latest/develop/tools/redis-for-vscode/).
                        

```python
res = index.search(Query('@brand:"Noka Bikes"'))
print(res)
# >>> Result{1 total, docs: [
# Document {
#   'id': 'bicycle:4',
#   'payload': None,
#   'json': '{
#       "brand":"Noka Bikes",
#       "model":"Kahuna",
#       "price":3200,
#       ...
#       "condition":"used"
#    }'
# }]}
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

search(

query: Union[str, Query], // The search query

query_params: Optional[Dict[str, Union[str, int, float]]] // Optional query parameters

) â 
Result // The search results

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

```node.js
result = await client.ft.search(
  'idx:bicycle',
  '@brand:"Noka Bikes"',
  {
    LIMIT: {
      from: 0,
      size: 10
    }
  }
);

console.log(JSON.stringify(result, null, 2));

/*
{
  "total": 1,
  "documents": [{
    "id": "bicycle:4",
    "value": {
      "brand": "Noka Bikes",
      "model": "Kahuna",
      "price": 3200,
      "description": "Whether you want to try your hand at XC racing or are looking for a lively trail bike that's just as inspiring on the climbs as it is over rougher ground, the Wilder is one heck of a bike built specifically for short women. Both the frames and components have been tweaked to include a womenâs saddle, different bars and unique colourway.",
      "condition": "used"
    }
  }]
}
*/
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

SEARCH(

index: RedisArgument, // The index name

query: RedisArgument, // The search query

options: FTSearchOptions // Optional search options

) â 
Promise<FTSearchResult> // The search results

[Node.js Quick-Start](https://redis.io/docs/latest/develop/clients/nodejs/)

```java
Query query5 = new Query("@brand:\"Noka Bikes\"");
    List<Document> result5 = jedis.ftSearch("idx:bicycle", query5).getDocuments();
    System.out.println(result5);
    // Prints: [id:bicycle:4, score: 1.0, payload:null,
    // properties:[$={"brand":"Noka Bikes","model":"Kahuna","price":3200,"description":"Whether you want to try your hand at XC racing or are looking for a lively trail bike that's just as inspiring on the climbs as it is over rougher ground, the Wilder is one heck of a bike built specifically for short women. Both the frames and components have been tweaked to include a womenâs saddle, different bars and unique colourway.","condition":"used"}]]
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

ftSearch(

indexName: String, // The index name

query: Query // The search query

) â 
SearchResult // The search results

ftSearch(

indexName: String, // The index name

query: String, // The search query

searchParams: FTSearchParams // Search parameters

) â 
SearchResult // The search results

[Java-Sync Quick-Start](https://redis.io/docs/latest/develop/clients/jedis/)

```go
exactMatchResult, err := rdb.FTSearch(
		ctx,
		"idx:bicycle",
		"@brand:\"Noka Bikes\"",
	).Result()

	if err != nil {
		panic(err)
	}

	fmt.Println(exactMatchResult)
	// >>> {1 [{bicycle:4 <nil> <nil> <nil> map[$:{"brand":"Noka Bikes"...
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

FTSearch(

ctx: context.Context,

index: string, // The index name

query: string // The search query

) â 
*FTSearchCmd // The search results

FTSearchWithArgs(

ctx: context.Context,

index: string, // The index name

query: string, // The search query

options: *FTSearchOptions // Search options

) â 
*FTSearchCmd // The search results

[Go Quick-Start](https://redis.io/docs/latest/develop/clients/go/)

```c#
var query4 = new Query("@brand:\"Noka Bikes\"");
        var res4 = ft.Search("idx:bicycle", query4).Documents;
        Console.WriteLine(string.Join("\n", res4.Select(x => x["json"])));
        // Prints: {"brand":"Moore PLC","model":"Award Race","price":3790.76,
        //          "description":"This olive folding bike features a carbon frame
        //          and 27.5 inch wheels. This folding bike is perfect for compact
        //          storage and transportation.","condition":"new"}
```

[FT.SEARCH](https://redis.io/docs/latest/commands/ft.search/)

Searches the index with a textual query, returning either documents or just ids

Search(

indexName: string, // The index name

q: Query // The search query

) â 
SearchResult // Search results

[C#-Sync (NRedisStack) Quick-Start](https://redis.io/docs/latest/develop/clients/dotnet/)

Please see the [query documentation](/docs/latest/develop/ai/search-and-query/query/) to learn how to make more advanced queries.

## Next steps

You can learn more about how to use Redis Open Source as a vector database in the following quick start guide:

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