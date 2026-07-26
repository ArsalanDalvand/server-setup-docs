---
title: "Run Redis Open Source on Docker"
source: "https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/docker/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/oss_and_stack/install/install-stack/docker"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Run Redis Open Source on Docker"
---
# Run Redis Open Source on Docker

How to run Redis Open Source using Docker

            Redis Open Source
          

## Install Docker

Follow the Docker installation instructions for your operating system:

[Linux](https://docs.docker.com/desktop/setup/install/linux/)
[macOS](https://docs.docker.com/docker-for-mac/install/)
[Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows)

Note:
  On Windows, make sure Docker is configured to run Linux-based containers.

## Run Redis Open Source on Docker

To start the Redis Open Source server using the `redis:<version>` image, run the following command in your terminal:

```bash
docker run -d --name redis -p 6379:6379 redis:<version>
```

## Connect with redis-cli

You can then connect to the server using `redis-cli`, just as you connect to any Redis instance.

If you donât have `redis-cli` installed locally, you can run it from the Docker container:

```bash
$ docker exec -it redis redis-cli
```

If you do have `redis-cli` installed locally, you can run it from your terminal:

```bash
$ redis-cli -h 127.0.0.1 -p 6379
```

## Use a local configuration file

By default, the Redis Docker containers use internal configuration files for Redis. To start Redis with local configuration file, you can do one of the following:

You can create your own Dockerfile that adds a `redis.conf` from the context into `/data/`, like so.

```
FROM redis
COPY redis.conf /usr/local/etc/redis/redis.conf
CMD [ "redis-server", "/usr/local/etc/redis/redis.conf" ]
```

Alternatively, you can specify something along the same lines with docker run options.

```bash
$ docker run -v /myredis/conf:/usr/local/etc/redis --name myredis redis redis-server /usr/local/etc/redis/redis.conf
```

where `/myredis/conf/` is a local directory containing your `redis.conf` file. Using this method means that there is no need for you to have a Dockerfile for your redis container.

The mapped directory should be writable, as depending on the configuration and mode of operation, Redis may need to create additional configuration files or rewrite existing ones.

## Use local storage for data persistence

To mount directories or files to your Docker container, specify -v to configure a local volume. This command stores all data in the local directory local-data:

```bash
$ docker run -v /local-data/:/data --name redis -p 6379:6379 redis:<version>
```

See the [official Redis page on Docker Hub](https://hub.docker.com/_/redis) for more options.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page