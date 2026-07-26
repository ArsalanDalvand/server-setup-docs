---
title: "Use Redis with the Spring cache abstraction"
source: "https://redis.io/docs/latest/integrate/spring-framework-cache/cache/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/spring-framework-cache/cache"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Use Redis with the Spring cache abstraction"
---
# Use Redis with the Spring cache abstraction

Plug Redis into your Spring application as a cache with minimal effort

Spring Data Redis implements the Spring framework's cache abstraction for Redis, which allows you to plug Redis into your Spring application with minimal effort.

Spring's cache abstraction applies cache-aside to methods, reducing executions by storing and reusing results. When a method is invoked, the abstraction checks if it's been called with the same arguments before. If so, it returns the cached result. If not, it invokes the method, caches the result, and returns it. This way, costly methods are invoked less often. Further details are in the [Spring cache abstraction documentation](https://docs.spring.io/spring-framework/reference/integration/cache.html).

## Get started

In a nutshell, you need to perform the following steps to use Redis as your cache storage:

[Configure the cache storage](https://docs.spring.io/spring-framework/reference/integration/cache/store-configuration.html) by using the [Redis cache manager](https://docs.spring.io/spring-data/redis/reference/redis/redis-cache.html) that is part of Spring Data.
Annotate a repository with your `@CacheConfig`.
Use the `@Cacheable` annotation on a repository method to cache the results of that method.

Here is an example:

```java
@CacheConfig("books")
public class BookRepositoryImpl implements BookRepository {

    @Cacheable
    public Book findBook(ISBN isbn) {...}
}
```

## Further readings

Please read the Spring framework's documentation to learn more about how to use the Redis cache abstraction for Spring:

[Spring cache abstraction](https://docs.spring.io/spring-framework/reference/integration/cache.html)
[Spring cache store configuration](https://docs.spring.io/spring-framework/reference/integration/cache/store-configuration.html)
[Spring Data Redis Cache](https://docs.spring.io/spring-data/redis/reference/redis/redis-cache.html)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page