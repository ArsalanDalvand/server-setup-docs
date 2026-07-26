---
title: "Redis Cloud quick start"
source: "https://redis.io/docs/latest/operate/rc/rc-quickstart/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/rc-quickstart"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud quick start"
---
# Redis Cloud quick start

            Redis Cloud
          

If you're new to Redis Cloud, this quick start helps you get up and running.

You'll learn how to:

Create an account and a free database

Connect to your database

If you already have an account, see [Create an Essentials database](/docs/latest/operate/rc/databases/create-database/create-essentials-database/) to create a Free 30 MB Essentials database. Free plans are a type of Essentials plans; this provides an easy upgrade path when you need it.

If you already have a database, see [Manage databases](/docs/latest/operate/rc/databases/).

## Create an account

To create a new account with a free database:

Go to the [Sign up](https://redis.io/try-free/) page.

There are two options available to get started with Redis Cloud:

Enter your information in the form and select Get Started.
Sign up with Google or Github.

After you enter your information, you should receive an activation email from Redis. Select Activate account to go to the Create your database page in the [Redis Cloud console](https://cloud.redis.io).

You'll go to the [create database](/docs/latest/operate/rc/databases/create-database/) page with the Free plan selected.

[](/docs/latest/images/rc/create-database-subscription-free.png)

Note:

If you want to create a different type of database, see the following guides:

[Create a Redis Flex database](/docs/latest/operate/rc/databases/create-database/create-flex-database/)
[Create an Essentials database](/docs/latest/operate/rc/databases/create-database/create-essentials-database/)
[Create a Pro database](/docs/latest/operate/rc/databases/create-database/create-pro-database-new/)

Redis will generate a database name for you. If you want to change it, you can do so in the Database name field.

[](/docs/latest/images/rc/create-database-free-settings.png)

Select the Database version you want to use.

Choose your Cloud vendor and Region. You can choose between Amazon Web Services (AWS), Google Cloud, and Microsoft Azure for the Cloud Vendor.

See [[operate/rc/supported-regions|Supported regions]] for a list of supported regions by cloud vendor.

Select Create database.

[](/docs/latest/images/rc/button-create-db.png)

When you create your database, there's a brief pause while your request is processed and then the Database details page appears.

In the upper corner, an icon shows the current status of the database.  If the icon shows an orange clock, this means your database is still being created and its status is pending.

[](/docs/latest/images/rc/icon-database-update-status-pending.png#no-click)
[](/docs/latest/images/rc/icon-database-status-active.png#no-click)

Once the database has been created, it becomes active and the status indicator switches to a green circle containing a checkmark.

When your new database becomes active, you're ready to connect to it.

## Connect to a database

At this point, you're viewing the Configuration details for your new database. Go to the Security section of the page.

To connect to your database, you need your username and password. Each database is protected by a Default user called `default` and a masked Default user password. Select Default user > Configure and then select the eye icon to view your password.

[](/docs/latest/images/rc/database-essentials-default-user.png)

Once you have the username and password, select Connect to open the connection wizard.

[](/docs/latest/images/rc/button-connect.png#no-click)

The connection wizard provides the following database connection methods:

[Redis Insight](#using-redisinsight)

[`redis-cli`](#using-rediscli) utility

[Redis client](#using-redis-client) for your preferred programming language

[](/docs/latest/images/rc/connection-wizard.png)

### Redis Insight

[Redis Insight](/docs/latest/develop/tools/insight/) is a free Redis GUI that lets you visualize your Redis data and learn more about Redis.

You can connect to your database with Redis Insight in two ways:

[Open your database in Redis Insight in your browser](#ri-browser).

[Download and Install Redis Insight](#ri-app) on Windows, macOS, and Linux.

#### Open in your browser

Note:
  Opening your database with Redis Insight in your browser is only available for Essentials databases. For all other databases, [Download and install Redis Insight](#ri-app) on your computer.

Select Launch Redis Insight web from the connection wizard to open it.

[](/docs/latest/images/rc/rc-ri-wizard-launch.png)

You can also select Launch from the database page under View and manage data with Redis Insight to open Redis Insight in your browser.

[](/docs/latest/images/rc/rc-ri-open.png)

Redis Insight will open in a new tab.

From there, you can:

Select Load sample data to add sample data into your database.

    
    	
    
    [](/docs/latest/images/rc/rc-ri-load-data.png)

Select Insights to learn how to use Redis.

    
    	
    
    [](/docs/latest/images/rc/rc-ri-explore-icon.png)

For more information on how to use Redis Insight in your browser, see [Open with Redis Insight on Redis Cloud](/docs/latest/operate/rc/databases/connect/insight-cloud/).

#### Install and open on your computer

In the connection wizard, under Redis Insight, select Download to download Redis Insight.

[Install Redis Insight](/docs/latest/develop/tools/insight/).

Once installed, select Open in desktop.

A pop-up asks if you wish to open the link with Redis Insight. Select Open Redis Insight to connect to your database with Redis Insight.

See the [Redis Insight docs](/docs/latest/develop/tools/insight/) for more info.

### Redis client

A Redis client is a software library or tool that enables applications to interact with a Redis server. Each client has its own syntax and installation process. For help with a specific client, see the client's documentation.

The connection wizard provides code snippets to connect to your database with the following programming languages:

.NET using [StackExchange.Redis/NRedisStack](/docs/latest/develop/clients/dotnet/)
node.js using [node-redis](/docs/latest/develop/clients/nodejs/)
Python using [[develop/clients/redis-py|redis-py]]
Java using [Jedis](/docs/latest/develop/clients/jedis/) and [Lettuce](/docs/latest/develop/clients/lettuce/)
Go using [go-redis](/docs/latest/develop/clients/go/)
PHP using [Predis](/docs/latest/develop/clients/php/)

[](/docs/latest/images/rc/connection-wizard-clients.png)

See [[develop/clients|Clients]] to learn how to connect with the official Redis clients.

See the following guides to get started with different Redis use cases:

[[develop/get-started/data-store|Data structure store]]
[[develop/get-started/document-database|Document database]]
[[develop/get-started/vector-database|Vector database]]
[[develop/get-started/rag|RAG with Redis]]
[Redis for AI](/docs/latest/develop/ai/)

### redis-cli

The [[develop/tools/cli|`redis-cli`]] utility is installed when you install Redis.  It provides a command-line interface that lets you work with your database using core [[commands|Redis commands]].

To run `redis-cli`, [[operate/oss_and_stack/install/install-stack|install Redis]] on your machine.

See [[develop/tools/cli|Redis CLI]] to learn how to use `redis-cli`.

## More info

[[develop/clients|Connect your application]]
[Import data](/docs/latest/operate/rc/databases/import-data/)
[Manage databases](/docs/latest/operate/rc/databases/)
[Data persistence](/docs/latest/operate/rc/databases/configuration/data-persistence/)
[[operate/rc/security|Secure your Redis Cloud database]]
[Back-up databases](/docs/latest/operate/rc/databases/back-up-data/)
[Monitor Redis Cloud performance](/docs/latest/operate/rc/databases/monitor-performance/)

## Continue learning with Redis University

See the [Get started with Redis Cloud learning path](https://university.redis.io/learningpath/lwkfbjbbqetoqc) for courses.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page