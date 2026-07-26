---
title: "Redis as a vector database quick start guide"
source: "https://redis.io/docs/latest/develop/get-started/vector-database/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/get-started/vector-database"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis as a vector database quick start guide"
---
# Redis as a vector database quick start guide

Understand how to use Redis as a vector database

This quick start guide helps you to:

Understand what a vector database is
Create a Redis vector database
Create vector embeddings and store vectors
Query data and perform a vector search

Note:

This guide uses [[develop/clients/redis-vl|RedisVL]],
which is a Python client library for Redis that is highly specialized for
vector processing. You may also be interested in the vector query examples
for our other client libraries:

[[develop/clients/redis-py/vecsearch|`redis-py` (Python)]]
[`NRedisStack`(C#/.NET)](/docs/latest/develop/clients/dotnet/nredisstack/vecsearch/)
[`node-redis` (JavaScript/Node.js)](/docs/latest/develop/clients/nodejs/vecsearch/)
[`jedis` (Java)](/docs/latest/develop/clients/jedis/vecsearch/)
[`go-redis` (Go)](/docs/latest/develop/clients/go/vecsearch/)

## Understand vector databases

Data is often unstructured, which means that it isn't described by a well-defined schema. Examples of unstructured data include text passages, images, videos, or audio. One approach to storing and searching through unstructured data is to use vector embeddings.

What are vectors? In machine learning and AI, vectors are sequences of numbers that represent data. They are the inputs and outputs of models, encapsulating underlying information in a numerical form. Vectors transform unstructured data, such as text, images, videos, and audio, into a format that machine learning models can process.

Why are they important? Vectors capture complex patterns and semantic meanings inherent in data, making them powerful tools for a variety of applications. They allow machine learning models to understand and manipulate unstructured data more effectively.
Enhancing traditional search. Traditional keyword or lexical search relies on exact matches of words or phrases, which can be limiting. In contrast, vector search, or semantic search, leverages the rich information captured in vector embeddings. By mapping data into a vector space, similar items are positioned near each other based on their meaning. This approach allows for more accurate and meaningful search results, as it considers the context and semantic content of the query rather than just the exact words used.

## Create a Redis vector database

You can use [[operate/oss_and_stack|Redis Open Source]] as a vector database. It allows you to:

Store vectors and the associated metadata within hashes or [JSON](/docs/latest/develop/data-types/json/) documents
Create and configure secondary indices for search
Perform vector searches
Update vectors and metadata
Delete and cleanup

The easiest way to get started is to use Redis Cloud:

Create a [free account](https://redis.com/try-free?utm_source=redisio&utm_medium=referral&utm_campaign=2023-09-try_free&utm_content=cu-redis_cloud_users).

Follow the instructions to create a free database.

This free Redis Cloud database comes out of the box with all the Redis Open Source features.

You can alternatively use the [[operate/oss_and_stack/install/install-stack|installation guides]] to install Redis on your local machine.

## Install the required Python packages

Create a Python virtual environment and install the following dependencies using `pip`:

`redis`: You can find further details about the `redis-py` client library in the [[develop/clients/redis-py|clients]] section of this documentation site.
`pandas`: Pandas is a data analysis library.
`sentence-transformers`: You will use the [SentenceTransformers](https://www.sbert.net/) framework to generate embeddings on full text.
`tabulate`: `pandas` uses `tabulate` to render Markdown.

You will also need the following imports in your Python code:

Language:

                        Python
                    

Foundational: Import required Python packages for vector database operations including redis, pandas, and sentence-transformers

```python
import json
import time

import numpy as np
import pandas as pd
import requests
import redis
from redis.commands.search.field import (
    NumericField,
    TagField,
    TextField,
    VectorField,
)
from redis.commands.search.index_definition import IndexDefinition, IndexType
from redis.commands.search.query import Query
from sentence_transformers import SentenceTransformer
```

```python

```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

## Connect

Connect to Redis. By default, Redis returns binary responses. To decode them, you pass the `decode_responses` parameter set to `True`:

Language:

                        Python
                    

Foundational: Connect to a Redis server with decode_responses enabled to receive decoded strings instead of bytes

```python
client = redis.Redis(host="localhost", port=6379, decode_responses=True)
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

Tip:
  Instead of using a local Redis server, you can copy and paste the connection details from the Redis Cloud database configuration page. Here is an example connection string of a Cloud database that is hosted in the AWS region `us-east-1` and listens on port 16379: `redis-16379.c283.us-east-1-4.ec2.cloud.redislabs.com:16379`. The connection string has the format `host:port`. You must also copy and paste the username and password of your Cloud database. The line of code for connecting with the default user changes then to `client = redis.Redis(host="redis-16379.c283.us-east-1-4.ec2.cloud.redislabs.com", port=16379, password="your_password_here", decode_responses=True)`.

## Prepare the demo dataset

This quick start guide also uses the bikes dataset. Here is an example document from it:

```json
{
  "model": "Jigger",
  "brand": "Velorim",
  "price": 270,
  "type": "Kids bikes",
  "specs": {
    "material": "aluminium",
    "weight": "10"
  },
  "description": "Small and powerful, the Jigger is the best ride for the smallest of tikes! ..."
}
```

The `description` field contains free-form text descriptions of bikes and will be used to create vector embeddings.

### 1. Fetch the demo data

You need to first fetch the demo dataset as a JSON array:

Language:

                        Python
                    

Foundational: Fetch the demo dataset as a JSON array to prepare data for vector database operations

```python
URL = ("https://raw.githubusercontent.com/bsbodden/redis_vss_getting_started"
       "/main/data/bikes.json"
       )
response = requests.get(URL, timeout=10)
bikes = response.json()
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

Inspect the structure of one of the bike JSON documents:

Language:

                        Python
                    

Foundational: Inspect the structure of a single document from the demo dataset to understand the data format

```python
json.dumps(bikes[0], indent=2)
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

### 2. Store the demo data in Redis

Now iterate over the `bikes`  array to store the data as [JSON](/docs/latest/develop/data-types/json/) documents in Redis by using the [JSON.SET](/docs/latest/commands/json.set/) command. The below code uses a [[develop/using-commands/pipelining|pipeline]] to minimize the network round-trip times:

Language:

                        Python
                    

Practical pattern: Store demo data as JSON documents in Redis using JSON.SET with pipelining to minimize network round-trips

```python
pipeline = client.pipeline()
for i, bike in enumerate(bikes, start=1):
    redis_key = f"bikes:{i:03}"
    pipeline.json().set(redis_key, "$", bike)
res = pipeline.execute()
# >>> [True, True, True, True, True, True, True, True, True, True, True]
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

Once loaded, you can retrieve a specific attribute from one of the JSON documents in Redis using a [JSONPath](https://goessner.net/articles/JsonPath/) expression:

Language:

                        Python
                    

Foundational: Retrieve a specific attribute from a JSON document in Redis using JSONPath expressions

```python
res = client.json().get("bikes:010", "$.model")
# >>> ['Summit']
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

### 3. Select a text embedding model

[HuggingFace](https://huggingface.co) has a large catalog of text embedding models that are locally servable through the `SentenceTransformers` framework. Here we use the [MS MARCO](https://microsoft.github.io/msmarco/) model that is widely used in search engines, chatbots, and other AI applications.

```python
from sentence_transformers import SentenceTransformer

embedder = SentenceTransformer('msmarco-distilbert-base-v4')
```

### 4. Generate text embeddings

Iterate over all the Redis keys with the prefix `bikes:`:

Language:

                        Python
                    

Foundational: Retrieve all Redis keys with a specific prefix to identify documents for processing

```python
keys = sorted(client.keys("bikes:*"))
# >>> ['bikes:001', 'bikes:002', ..., 'bikes:011']
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

Use the keys as input to the [JSON.MGET](/docs/latest/commands/json.mget/) command, along with the `$.description` field, to collect the descriptions in a list. Then, pass the list of descriptions to the `.encode()` method:

Language:

                        Python
                    

Create embeddings: Generate text embeddings from document descriptions using SentenceTransformers model to create vector representations

```python
descriptions = client.json().mget(keys, "$.description")
descriptions = [item for sublist in descriptions for item in sublist]
embedder = SentenceTransformer("msmarco-distilbert-base-v4")
embeddings = embedder.encode(descriptions).astype(np.float32).tolist()
VECTOR_DIMENSION = len(embeddings[0])
# >>> 768
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

Insert the vectorized descriptions to the bike documents in Redis using the [JSON.SET](/docs/latest/commands/json.set/) command. The following command inserts a new field into each of the documents under the JSONPath `$.description_embeddings`. Once again, do this using a pipeline to avoid unnecessary network round-trips:

Language:

                        Python
                    

Practical pattern: Insert generated vector embeddings into JSON documents using JSON.SET with pipelining for efficient batch updates

```python
pipeline = client.pipeline()
for key, embedding in zip(keys, embeddings):
    pipeline.json().set(key, "$.description_embeddings", embedding)
pipeline.execute()
# >>> [True, True, True, True, True, True, True, True, True, True, True]
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

Inspect one of the updated bike documents using the [JSON.GET](/docs/latest/commands/json.get/) command:

Language:

                        Python
                    

Foundational: Inspect an updated document containing both original data and generated vector embeddings using JSON.GET

```python
res = client.json().get("bikes:010")
# >>>
# {
#   "model": "Summit",
#   "brand": "nHill",
#   "price": 1200,
#   "type": "Mountain Bike",
#   "specs": {
#     "material": "alloy",
#     "weight": "11.3"
#   },
#   "description": "This budget mountain bike from nHill performs well..."
#   "description_embeddings": [
#     -0.538114607334137,
#     -0.49465855956077576,
#     -0.025176964700222015,
#     ...
#   ]
# }
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

Note:
  When storing a vector embedding within a JSON document, the embedding is stored as a JSON array. In the example above, the array was shortened considerably for the sake of readability.

## Create an index

### 1. Create an index with a vector field

You must create an index to query document metadata or to perform vector searches. Use the [FT.CREATE](/docs/latest/commands/ft.create/) command:

Language:

                        >_ Redis CLI
                    

                        Python
                    

Foundational: Create an index on JSON documents with vector field using FT.CREATE with FLAT indexing and COSINE distance metric

```
FT.CREATE idx:bikes_vss ON JSON
  PREFIX 1 bikes: SCORE 1.0
  SCHEMA
    $.model AS model TEXT WEIGHT 1.0 NOSTEM
    $.brand AS brand TEXT WEIGHT 1.0 NOSTEM
    $.price AS price NUMERIC
    $.type TAG SEPARATOR ","
    $.description AS description TEXT WEIGHT 1.0
    $.description_embeddings AS vector VECTOR FLAT 6 TYPE FLOAT32 DIM 768 DISTANCE_METRIC COSINE
```

[Redis CLI guide](https://redis.io/docs/latest/develop/tools/cli/)

                            Also, check out our other client tools
                            [Redis Insight](https://redis.io/docs/latest/develop/tools/insight/)
                            and
                            [Redis for VS Code](https://redis.io/docs/latest/develop/tools/redis-for-vscode/).
                        

```python
schema = (
    TextField("$.model", no_stem=True, as_name="model"),
    TextField("$.brand", no_stem=True, as_name="brand"),
    NumericField("$.price", as_name="price"),
    TagField("$.type", as_name="type"),
    TextField("$.description", as_name="description"),
    VectorField(
        "$.description_embeddings",
        "FLAT",
        {
            "TYPE": "FLOAT32",
            "DIM": VECTOR_DIMENSION,
            "DISTANCE_METRIC": "COSINE",
        },
        as_name="vector",
    ),
)
definition = IndexDefinition(prefix=["bikes:"], index_type=IndexType.JSON)
res = client.ft("idx:bikes_vss").create_index(fields=schema, definition=definition)
# >>> 'OK'
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

Here is a breakdown of the `VECTOR` field definition:

`$.description_embeddings AS vector`: The vector field's JSON path and its field alias `vector`.
`FLAT`: Specifies the indexing method, which is either a flat index or a hierarchical navigable small world graph ([HNSW](https://arxiv.org/abs/1603.09320)).
`TYPE FLOAT32`: Sets the float precision of a vector component, in this case a 32-bit floating point number.
`DIM 768`: The length or dimension of the embeddings, determined by the chosen embedding model.
`DISTANCE_METRIC COSINE`: The chosen distance function: [cosine distance](https://en.wikipedia.org/wiki/Cosine_similarity).

You can find further details about all these options in the [vector reference documentation](/docs/latest/develop/ai/search-and-query/vectors/).

### 2. Check the state of the index

As soon as you execute the [FT.CREATE](/docs/latest/commands/ft.create/) command, the indexing process runs in the background. In a short time, all JSON documents should be indexed and ready to be queried. To validate that, you can use the [FT.INFO](/docs/latest/commands/ft.info/) command, which provides details and statistics about the index. Of particular interest are the number of documents successfully indexed and the number of failures:

Language:

                        >_ Redis CLI
                    

                        Python
                    

Practical pattern: Check the state and statistics of an index using FT.INFO to verify successful indexing of documents

```
FT.INFO idx:bikes_vss
```

[Redis CLI guide](https://redis.io/docs/latest/develop/tools/cli/)

                            Also, check out our other client tools
                            [Redis Insight](https://redis.io/docs/latest/develop/tools/insight/)
                            and
                            [Redis for VS Code](https://redis.io/docs/latest/develop/tools/redis-for-vscode/).
                        

```python
info = client.ft("idx:bikes_vss").info()
num_docs = info["num_docs"]
indexing_failures = info["hash_indexing_failures"]
# print(f"{num_docs} documents indexed with {indexing_failures} failures")
# >>> 11 documents indexed with 0 failures
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

## Perform vector searches

This quick start guide focuses on vector search. However, you can learn more about how to query based on document metadata in the [[develop/get-started/document-database|document database quick start guide]].

### 1. Embed your queries

The following code snippet shows a list of text queries you will use to perform vector search in Redis:

Language:

                        Python
                    

Foundational: Define a list of text queries to use for vector search operations

```python
queries = [
    "Bike for small kids",
    "Best Mountain bikes for kids",
    "Cheap Mountain bike for kids",
    "Female specific mountain bike",
    "Road bike for beginners",
    "Commuter bike for people over 60",
    "Comfortable commuter bike",
    "Good bike for college students",
    "Mountain bike for beginners",
    "Vintage bike",
    "Comfortable city bike",
]
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

First, encode each input query as a vector embedding using the same SentenceTransformers model:

Language:

                        Python
                    

Create query embeddings: Encode text queries as vector embeddings using the same SentenceTransformers model used for document embeddings

```python
encoded_queries = embedder.encode(queries)
len(encoded_queries)
# >>> 11
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

Tip:
  It is vital that you use the same embedding model to embed your queries as you did your documents. Using a different model will result in poor semantic search results or error.

### 2. K-nearest neighbors (KNN) search

The KNN algorithm calculates the distance between the query vector and each vector in Redis based on the chosen distance function. It then returns the top K items with the smallest distances to the query vector. These are the most semantically similar items.

Now construct a query to do just that:

```python
query = (
    Query('(*)=>[KNN 3 @vector $query_vector AS vector_score]')
     .sort_by('vector_score')
     .return_fields('vector_score', 'id', 'brand', 'model', 'description')
     .dialect(2)
)
```

Let's break down the above query template:

The filter expression `(*)` means `all`. In other words, no filtering was applied. You could replace it with an expression that filters by additional metadata.
The `KNN` part of the query searches for the top 3 nearest neighbors.
The query vector must be passed in as the param `query_vector`.
The distance to the query vector is returned as `vector_score`.
The results are sorted by this `vector_score`.
Finally, it returns the fields `vector_score`,  `id`, `brand`, `model`, and `description` for each result.

Note:
  To utilize a vector query with the [`FT.SEARCH`](/docs/latest/commands/ft.search/) command, you must specify DIALECT 2 or greater.

You must pass the vectorized query as a byte array with the param name `query_vector`. The following code creates a Python NumPy array from the query vector and converts it into a compact, byte-level representation that can be passed as a parameter to the query:

```python
for i, encoded_query in enumerate(encoded_queries):
    result = client.ft('idx:bikes_vss').search(
        query,
        {
          'query_vector': np.array(encoded_query, dtype=np.float32).tobytes()
        }
    )
    print(f"Results for query '{queries[i]}':")
    for doc in result.docs:
        print(f"\t{doc.id}: {doc.brand} {doc.model}")
```

With the template for the query in place, you can execute all queries in a loop. Notice that the script calculates the `vector_score` for each result as `1 - doc.vector_score`. Because the cosine distance is used as the metric, the items with the smallest distance are closer and, therefore, more similar to the query.

Then, loop over the matched documents and create a list of results that can be converted into a Pandas table to visualize the results:

Language:

                        Python
                    

Query data: Execute multiple vector search queries and format results into a Pandas table for visualization and analysis

```python
def create_query_table(query, queries, encoded_queries, extra_params=None):
    """
    Creates a query table.
    """
    results_list = []
    for i, encoded_query in enumerate(encoded_queries):
        result_docs = (
            client.ft("idx:bikes_vss")
            .search(
                query,
                {"query_vector": np.array(encoded_query, dtype=np.float32).tobytes()}
                | (extra_params if extra_params else {}),
            )
            .docs
        )
        for doc in result_docs:
            vector_score = round(1 - float(doc.vector_score), 2)
            results_list.append(
                {
                    "query": queries[i],
                    "score": vector_score,
                    "id": doc.id,
                    "brand": doc.brand,
                    "model": doc.model,
                    "description": doc.description,
                }
            )

    # Optional: convert the table to Markdown using Pandas
    queries_table = pd.DataFrame(results_list)
    queries_table.sort_values(
        by=["query", "score"], ascending=[True, False], inplace=True
    )
    queries_table["query"] = queries_table.groupby("query")["query"].transform(
        lambda x: [x.iloc[0]] + [""] * (len(x) - 1)
    )
    queries_table["description"] = queries_table["description"].apply(
        lambda x: (x[:497] + "...") if len(x) > 500 else x
    )
    return queries_table.to_markdown(index=False)
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

The query results show the individual queries' top three matches (our K parameter) along with the bike's id, brand, and model for each query.

For example, for the query "Best Mountain bikes for kids", the highest similarity score (`0.54`) and, therefore the closest match was the 'Nord' brand 'Chook air 5' bike model, described as:

The Chook Air 5 gives kids aged six years and older a durable and uberlight mountain bike for their first experience on tracks and easy cruising through forests and fields. The lower top tube makes it easy to mount and dismount in any situation, giving your kids greater safety on the trails. The Chook Air 5 is the perfect intro to mountain biking.

From the description, this bike is an excellent match for younger children, and the embeddings accurately captured the semantics of the description.

Language:

                        Python
                    

Semantic search: Execute K-nearest neighbors vector search using FT.SEARCH with KNN algorithm to find semantically similar documents

```python
query = (
    Query("(*)=>[KNN 3 @vector $query_vector AS vector_score]")
    .sort_by("vector_score")
    .return_fields("vector_score", "id", "brand", "model", "description")
    .dialect(2)
)

table = create_query_table(query, queries, encoded_queries)
print(table)
# >>> | Best Mountain bikes for kids     |    0.54 | bikes:003...
```

[Python Quick-Start](https://redis.io/docs/latest/develop/clients/redis-py/)

[](https://github.com/redis/redis-py/tree/master/doctests/search_vss.py)

query
score
id
brand
model
description

Best Mountain bikes for kids
0.54
bikes:003
Nord
Chook air 5
The Chook Air 5  gives kids aged six years and older a durable and uberlight mountain bike for their first experience on tracks and easy cruising through forests and fields. The lower  top tube makes it easy to mount and dismount in any situation, giving your kids greater safety on the trails. The Chook Air 5 is the perfect intro to mountain biking.

0.51
bikes:010
nHill
Summit
This budget mountain bike from nHill performs well both on bike paths and on the trail. The fork with 100mm of travel absorbs rough terrain. Fat Kenda Booster tires give you grip in corners and on wet trails. The Shimano Tourney drivetrain offered enough gears for finding a comfortable pace to ride uphill, and the Tektro hydraulic disc brakes break smoothly. Whether you want an affordable bike that you can take to work, but also take trail riding on the weekends or youâre just after a stable,...

0.46
bikes:001
Velorim
Jigger
Small and powerful, the Jigger is the best ride for the smallest of tikes! This is the tiniest kidsâ pedal bike on the market available without a coaster brake, the Jigger is the vehicle of choice for the rare tenacious little rider raring to go. We say rare because this smokinâ little bike is not ideal for a nervous first-time rider, but itâs a true giddy up for a true speedster. The Jigger is a 12 inch lightweight kids bicycle and it will meet your little oneâs need for speed. Itâs a single...

## Next steps

You can learn more about the query options, such as filters and vector range queries, by reading the [vector reference documentation](/docs/latest/develop/ai/search-and-query/vectors/).
The complete [Redis Search documentation](/docs/latest/develop/ai/search-and-query/) might be interesting for you.
If you want to follow the code examples more interactively, then you can use the [Jupyter notebook](https://github.com/redis-developer/redis-ai-resources/blob/main/python-recipes/vector-search/00_redispy.ipynb) that inspired this quick start guide.
If you want to see more advanced examples of a Redis vector database in action, visit the [Redis AI Resources](https://github.com/redis-developer/redis-ai-resources) page on GitHub.

## Continue learning with Redis University

See the [Redis vector database course](https://university.redis.io/course/7e2qbbeg963twz) for learning modules.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page