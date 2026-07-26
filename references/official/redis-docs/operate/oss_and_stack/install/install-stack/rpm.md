---
title: "Install Redis Open Source on Linux"
source: "https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/rpm/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/oss_and_stack/install/install-stack/rpm"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install Redis Open Source on Linux"
---
# Install Redis Open Source on Linux

How to install Redis Open Source using RPM

            Redis Open Source
          

## Install Redis Open Source on Rocky Linux 8 and 9, or AlmaLinux 8 and 9 using RPM

Follow these steps to install Redis Open Source.

Create the file `/etc/yum.repos.d/redis.repo` with the following contents.

For Rocky Linux 9 and AlmaLinux 9

```ini
[Redis]
name=Redis
baseurl=http://packages.redis.io/rpm/rockylinux9
enabled=1
gpgcheck=1
```

For Rocky Linux 8 and AlmaLinux 8

```ini
[Redis]
name=Redis
baseurl=http://packages.redis.io/rpm/rockylinux8
enabled=1
gpgcheck=1
```

Run the following commands:

```bash
curl -fsSL https://packages.redis.io/gpg > /tmp/redis.key
sudo rpm --import /tmp/redis.key
sudo yum install redis
```

Redis will not start automatically, nor will it start at boot time. To do this, run the following commands.

```bash
sudo systemctl enable redis
sudo systemctl start redis
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page