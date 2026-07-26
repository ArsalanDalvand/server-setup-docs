---
title: "Install Redis Stack on Linux"
source: "https://redis.io/docs/latest/operate/oss_and_stack/install/archive/install-stack/linux/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/oss_and_stack/install/archive/install-stack/linux"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install Redis Stack on Linux"
---
# Install Redis Stack on Linux

How to install Redis Stack on Linux

            Redis Open Source
          

These docs apply to Redis Community Edition and Redis Stack version 7.x.

Learn how to install Redis Stack on Linux from the official APT repository or RPM feed, or with Snap or AppImage.

## From the official Ubuntu/Debian APT Repository

See [this page](https://redis.io/downloads/#redis-stack-downloads) for a complete list of supported Ubuntu/Debian platforms.
Add the repository to the APT index, update it, and install Redis Stack:

```bash
sudo apt-get install lsb-release curl gpg
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
sudo chmod 644 /usr/share/keyrings/redis-archive-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
sudo apt-get update
sudo apt-get install redis-stack-server
```

Redis will not start automatically, nor will it start at boot time. To do this, run the following commands.

```bash
sudo systemctl enable redis-stack-server
sudo systemctl start redis-stack-server
```

## From the official Red Hat/Rocky RPM Feeds

See [this page](https://redis.io/downloads/#redis-stack-downloads) for a complete list of supported Red Hat/Rocky platforms.
Follow these steps to install Redis Stack.

Create the file `/etc/yum.repos.d/redis.repo` with the following contents.

```bash
[Redis]
name=Redis
baseurl=http://packages.redis.io/rpm/rhel9 # replace rhel9 with the appropriate value for your platform
enabled=1
gpgcheck=1
```

Run the following commands:

```bash
curl -fsSL https://packages.redis.io/gpg > /tmp/redis.key
sudo rpm --import /tmp/redis.key
sudo yum install epel-release
sudo yum install redis-stack-server
```

Redis will not start automatically, nor will it start at boot time. To do this, run the following commands.

```bash
sudo systemctl enable redis-stack-server
sudo systemctl start redis-stack-server
```

## On Ubuntu with Snap

First, download the latest Redis Stack snap package from [this page](https://redis.io/downloads/).

To install, run:

```bash
sudo apt update
sudo apt install redis-tools
sudo snap install --dangerous --classic <snapname.snap>
```

Redis will not start automatically, nor will it start at boot time. To start `redis-stack-server` in the foreground, run the command:

```bash
sudo snap run redis-stack-server
```

To stop Redis, enter `Ctrl-C`.

Follow these steps to integrate Redis Stack with `systemd` so you can start/stop in/from the background:

Edit the `/etc/systemd/system/redis-stack-server.service` file and enter the following information:

```text
[Unit]
Description=Redis Stack Server
After=network.target

[Service]
ExecStart=/usr/bin/snap run redis-stack-server
Restart=always
User=root
Group=root

[Install]
WantedBy=multi-user.target
```

Run the following commands.

```bash
sudo systemctl daemon-reload
sudo systemctl start redis-stack-server
sudo systemctl enable redis-stack-server
```

If your Linux distribution does not currently have Snap installed, you can install it using the instructions described  [here](https://snapcraft.io/docs/installing-snapd). Then, download the appropriate from the [downloads page](https://redis.io/downloads/).

## On Ubuntu with AppImage

Fuse needs to be installed before proceeding. Install it as follows.

```bash
sudo apt update
sudo apt install fuse
```

Next, download the latest Redis Stack AppImage package from [this page](https://redis.io/downloads/).

To run the image, execute these commands:

```bash
sudo apt update
sudo apt install redis-tools
chmod a+x <AppImageFile> # replace AppImageFile with the name of your downloaded file
./<AppImageFile>
```

This will start Redis in the foreground. To stop Redis, enter `Ctrl-C`.

Follow these steps to integrate Redis Stack with `systemd` so you can start/stop in/from the background:

Edit the `/etc/systemd/system/redis-appimage.service` file and enter the following information:

```text
[Unit]
Description=Redis Server (AppImage)
After=network.target

[Service]
ExecStart=/path/to/your/<AppImageFile> --daemonize no
Restart=always
User=redis-user   # replace with an existing user or create a new one
Group=redis-group # replace with an existing group or create a new one

[Install]
WantedBy=multi-user.target
```

Run the following commands.

```bash
sudo systemctl daemon-reload
sudo systemctl start redis-appimage
sudo systemctl enable redis-appimage
```

## Starting and stopping Redis Stack in the background

You can start the Redis server as a background process using the `systemctl` command. This only applies to Ubuntu/Debian when installed using `apt`, and Red Hat/Rocky when installed using `yum`.

```bash
sudo systemctl start redis-stack-server
```

To stop the service, use:

```bash
sudo systemctl stop redis-stack-server
```

## Connect to Redis

Once Redis is running, you can test it by running `redis-cli`:

```bash
redis-cli
```

Test the connection with the `ping` command:

```bash
127.0.0.1:6379> ping
PONG
```

You can also test that your Redis server is running using
[Redis Insight](/docs/latest/develop/tools/insight/).

## Next steps

Once you have a running Redis instance, you may want to:

Try the [[develop/tools/cli|Redis CLI tutorial]]
Connect using one of the [[develop/clients|Redis clients]]
[[operate/oss_and_stack/install/archive/install-redis#install-redis-properly|Install Redis properly]]
for production use.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page