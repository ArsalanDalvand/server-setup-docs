---
title: "Install Redis Open Source on macOS"
source: "https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/homebrew/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/oss_and_stack/install/install-stack/homebrew"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install Redis Open Source on macOS"
---
# Install Redis Open Source on macOS

How to install Redis Open Source on macOS using Homebrew

            Redis Open Source
          

## Install Redis Open Source on macOS using Homebrew

Note:
  Installation using Homebrew is only supported on macOS.

To install Redis Open Source on macOS, use [Homebrew](https://brew.sh/).
Make sure that you have [Homebrew installed](https://docs.brew.sh/Installation) before starting on the installation instructions below.

## Remove any existing Redis installation files

If you had previously installed Redis on your system using the default Homebrew formula called "redis", you need to remove it before installing Redis Open Source 8.x.

Follow these steps to remove any existing Redis installation files:

Uninstall Redis:

```bash
brew uninstall redis
```

Next check if the `redis.conf` file is still installed:

```bash
ls -l $(brew --prefix)/etc/redis.conf
```

If you get output similar to the following, then itâs still there:

```bash
-rw-r--r--@ 1 user  admin  122821  2 Oct 16:07 /opt/homebrew/etc/redis.conf
```

Run this command to remove the file:

```bash
rm -iv $(brew --prefix)/etc/redis.conf
```

Next, follow the instructions in the [next section](#install-using-homebrew) to install Redis Open Source 8.x using the Redis Homebrew cask.

## Install using Homebrew

First, tap the Redis Homebrew cask:

```bash
brew tap redis/redis
```

Next, run `brew install`:

```bash
brew install --cask redis
```

## Run Redis

If this is the first time you've installed Redis on your system, you need to be sure that your `PATH` variable includes the Redis installation location. This location is either `/opt/homebrew/bin` for Apple silicon Macs or `/usr/local/bin` for Intel-based Macs.

To check this, run:

```bash
echo $PATH
```

Next, confirm that the output contains `/opt/homebrew/bin` (Apple silicon Macs) or `/usr/local/bin` (Intel Mac). If neither `/opt/homebrew/bin` nor `/usr/local/bin` are in the output, add them.

Open the file `~/.bashrc` or `~/.zshrc` (depending on your shell), and add the following line.

```bash
export PATH=$(brew --prefix)/bin:$PATH
```

You can now start the Redis server as follows:

```bash
redis-server $(brew --prefix)/etc/redis.conf
```

The server will run in the background.

Note:
  Because Redis is installed using a Homebrew cask with the `brew tap` command, it will not be integrated with the `brew services` command.

## Connect to Redis

Once Redis is running, you can test it by running `redis-cli`:

```bash
redis-cli
```

Test the connection with the `ping` command:

```bash
127.0.0.1:6379> PING
PONG
```

### Verify that all modules are loaded correctly

If you had previously installed earlier versions of Redis using Homebrew, for example 7.2.x or 7.4.x, you should test to see if all the modules are loaded correctly by running the following command. Your output should look similar to the following:

```bash
$ redis-cli MODULE LIST
1) 1) "name"
   2) "bf"
   3) "ver"
   4) (integer) 80200
   5) "path"
   6) "/usr/local/lib/redis/modules//redisbloom.so"
   7) "args"
   8) (empty array)
2) 1) "name"
   2) "timeseries"
   3) "ver"
   4) (integer) 80200
   5) "path"
   6) "/usr/local/lib/redis/modules//redistimeseries.so"
   7) "args"
   8) (empty array)
3) 1) "name"
   2) "search"
   3) "ver"
   4) (integer) 80201
   5) "path"
   6) "/usr/local/lib/redis/modules//redisearch.so"
   7) "args"
   8) (empty array)
4) 1) "name"
   2) "vectorset"
   3) "ver"
   4) (integer) 1
   5) "path"
   6) ""
   7) "args"
   8) (empty array)
5) 1) "name"
   2) "ReJSON"
   3) "ver"
   4) (integer) 80200
   5) "path"
   6) "/usr/local/lib/redis/modules//rejson.so"
   7) "args"
   8) (empty array)
```

## Stop Redis

Run the following command:

```bash
redis-cli SHUTDOWN
```

## Uninstall Redis Open Source

To uninstall Redis, run:

```bash
brew uninstall redis
brew untap redis/redis
```

## Next steps

Once you have a running Redis instance, you may want to:

Try the [[develop/tools/cli|Redis CLI tutorial]]
Connect using one of the [[develop/clients|Redis clients]]
Connect using [Redis Insight](/docs/latest/develop/tools/insight/)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page