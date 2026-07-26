---
title: "Get started with Flex databases for Redis Software"
source: "https://redis.io/docs/latest/operate/rs/flex/get-started/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/flex/get-started"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Get started with Flex databases for Redis Software"
---
# Get started with Flex databases for Redis Software

Configure Flex databases on your Redis Software cluster.

            Redis Software
          

This page guides you through a quick setup of [[operate/rs/flex|Flex]] with a single node for testing and demo purposes.

For production environments, see [[operate/rs/flex/plan|Plan a Flex deployment for Redis Software]] and more detailed installation instructions in the [[operate/rs/installing-upgrading|install and setup]] section.

To set up a Redis Software cluster using Flex with a single node:

Review the [version requirements](#version-requirements).

[Install Redis Software](#install-redis-software) or run it in a [Docker container](#dockerbased-installation).

[Prepare and format flash memory](#prepare-and-format-flash-memory).

[Set up a cluster](#setup-cluster-flash) withÂ flash storage.

[Create a new database](#create-a-database) with flash enabled.

[Connect to your newÂ database](#connect-to-your-database).

## Version requirements

To create Flex databases, you need:

Redis Software cluster version 8.0.2-17 or later

Redis database version 8.2 or later

## Install Redis Software

### Bare metal, VM, Cloud instance

To install on bare metal, a virtual machine, or a Cloud instance:

Download the Redis Software binaries from the [download center](https://redis.io/downloads/#Redis_Software).

Upload the binaries to a Linux-based operating system.

Extract the image:

```sh
tar -vxf <downloaded tar file name>
```

After the `tar` command completes, you can find a new `install.sh` script in the current directory:

```sh
sudo ./install.sh -y
```

### Docker-based installation

For testing purposes, you can run a Redis Software Docker container:

```sh
docker run -d --cap-add sys_resource --name rp -p 8443:8443 -p 12000:12000 redislabs/redis:latest
```

## Prepare and format flash memory

After you [install Redis Software](#install-redis-software), use the `prepare_flash` script to prepare and format flash memory:

```sh
sudo /opt/redislabs/sbin/prepare_flash.sh
```

This script finds unformatted disks and mounts them as RAID partitions in `/var/opt/redislabs/flash`.

To verify the disk configuration, run:

```sh
sudo lsblk
```

## Set up a clusterÂ with flash storage

Direct your browser to `https://localhost:8443` on the host machine to
see the Redis Software Cluster Manager UI.

Note:
  Depending on your browser, you may see a certificate error.
Choose "continue to the website" to go to the setup screen.

Click Create new cluster.

Set up account credentials for a cluster administrator, then click Next to proceed to cluster setup.

Enter your cluster license key if you have one. Otherwise, the cluster uses the trial version.

Provide a cluster FQDN such as `mycluster.local`, then click Next.

In the Storage configuration section, turn on the Enable flash storage toggle.

Click Create cluster.

Click OK to confirm that you are aware of the replacement of the HTTPS TLS certificate on the node, and proceed through the browser warning.

## Create a database

On the Databases screen:

Select Quick database.

Verify Flash is selected for Runs on.

[](/docs/latest/images/rs/screenshots/databases/quick-db-flash-7-8-2.png)

Enter `12000` for the endpoint Port number.

Optionally, select Full options to see available alerts.

Click Create.

You now have a Flex database.

## Connect to your database

After you create the database, you can connect to it and store data. See [Test client connection](/docs/latest/operate/rs/databases/connect/test-client-connectivity/) for connection options and examples.

## Next steps

To see the true performance and scaleÂ of Flex, you must tune your I/O path and set the flash path to the mounted path of SSD or NVMe flash memory.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page