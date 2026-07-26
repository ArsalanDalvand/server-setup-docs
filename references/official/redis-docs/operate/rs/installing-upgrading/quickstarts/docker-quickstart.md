---
title: "Docker quickstart for Redis Software"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/quickstarts/docker-quickstart/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/quickstarts/docker-quickstart"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Docker quickstart for Redis Software"
---
# Docker quickstart for Redis Software

Set up a development or test deployment of Redis Software using Docker.

            Redis Software
          

Warning:
  Docker containers are currently only supported for development and test environments, not for production. Use [Redis Enterprise on Kubernetes](/docs/latest/operate/kubernetes/) for a supported containerized deployment.

For testing purposes, you can run Redis Software on Docker containers on
Linux, Windows, or MacOS.
The [Redis Software container](https://hub.docker.com/r/redislabs/redis/)
acts as a node in a cluster.

To get started with a single Redis Software container:

[Install Docker](#install-docker) for your operating system

[Run the Redis Software Docker container](#run-the-container)

[Set up a cluster](#set-up-a-cluster)

[Create a new database](#create-a-database)

[Connect to your database](#connect-to-your-database)

## Install Docker

Follow the Docker installation instructions for your operating system:

[Linux](https://docs.docker.com/install/#supported-platforms)
[MacOS](https://docs.docker.com/docker-for-mac/install/)
[Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows)

## Run the container

To download and start the Redis Software Docker container, run the following
[`docker run`](https://docs.docker.com/engine/reference/commandline/run/) command in the terminal or command line for your operating system.

Note:
  On Windows, make sure Docker is configured to run Linux-based containers.

```sh
docker run -d --cap-add sys_resource --name RE -p 8443:8443 -p 9443:9443 -p 12000:12000 redislabs/redis
```

The example command runs the Docker container with Redis Software on `localhost` and opens the following ports:

Port 8443 for HTTPS connections

Port 9443 for [REST API](/docs/latest/operate/rs/references/rest-api/) connections

Port 12000 configured Redis database port allowing client connections

You can publish other [[operate/rs/networking/port-configurations|ports]]
with `-p <host_port>:<container_port>` or use the `--network host` option to open all ports to the host network.

## Set up a cluster

In a browser, go to `https://<name-or-IP-address-of-the-machine-with-Redis-Enterprise-Software-installed>:8443` to access the Cluster Manager UI. If you use a browser on the host machine, you can also access the Cluster Manager UI at `https://localhost:8443`.

The cluster generates self-signed TLS certificates to secure the connection. Because these self-signed certificates are unknown to the browser, you must accept them before you proceed.

If the server does not show the sign-in screen, try again after a few minutes.

Select Create new cluster.

[](/docs/latest/images/rs/screenshots/cluster/setup/create-cluster.png)

Enter an email and password for the administrator account, then select Next to proceed to cluster setup.

[](/docs/latest/images/rs/screenshots/cluster/setup/admin-credentials.png)

You can also use these credentials to connect to the [REST API](/docs/latest/operate/rs/references/rest-api/).

Enter your cluster license key if you have one. Otherwise, a trial version is installed.

[](/docs/latest/images/rs/screenshots/cluster/setup/cluster-license-key.png)

In the Configuration section, enter a cluster FQDN such as `cluster.local`, then select Next.

[](/docs/latest/images/rs/screenshots/cluster/setup/config-cluster.png)

Warning:
  If the FQDN is `cluster.local`, you cannot configure DNS. You cannot change the FQDN after cluster creation.

On the node setup screen, select Create cluster to accept the defaults.

[](/docs/latest/images/rs/screenshots/cluster/setup/node-settings.png)

Select OK to acknowledge the replacement of the HTTPS TLS certificate on the node.  If you receive a browser warning, you can proceed safely.

[](/docs/latest/images/rs/screenshots/cluster/setup/https-page-refresh-modal.png)

## Create a database

On the Databases screen, select Quick database.

[](/docs/latest/images/rs/screenshots/databases/db-screen.png)

Enter 12000 for the Port.

If port 12000 is not available, enter any available port number between 10000 to 19999 or leave it blank to let the cluster assign a port number for you. You will use this port number to connect to the database.

[](/docs/latest/images/rs/screenshots/databases/quick-db-7-8-2.png)

Select Create to create your database.

When you see Database active appear on the database configuration screen, the database is activated and ready for you to use.

[](/docs/latest/images/rs/icons/db-active-icon.png)

You now have a Redis database!

Note:
  If you cannot activate the database because of a memory limitation,
make sure that Docker has at least 4 GB of memory allocated in the Advanced section of Docker Settings.

## Connect to your database

After you create the Redis database, you can connect to it to begin storing data.

### Use redis-cli inside Docker

Every installation of Redis Software includes the command-line tool [`redis-cli`](/docs/latest/operate/rs/references/cli-utilities/redis-cli/) to interact with your Redis database. You can use `redis-cli` to connect to your database from within the same Docker network.

Use [`docker exec`](https://docs.docker.com/engine/reference/commandline/exec/) to start an interactive `redis-cli` session in the running Redis Software container:

```sh
$ docker exec -it <container_name_or_ID> redis-cli -h <host_or_IP> -p <port>
127.0.0.1:12000> SET key1 123
OK
127.0.0.1:12000> GET key1
"123"
```

To find the container name or ID, use [`docker ps`](https://docs.docker.com/reference/cli/docker/container/ls/) to list running containers.

### Connect from the host environment

The database you created uses port `12000`, which is also mapped from the Docker container back to the host environment. This lets you use any method you have available locally to [connect to a Redis database](/docs/latest/operate/rs/databases/connect/). Use `localhost` as the `host` and `12000` as the port.

## Test different topologies

Warning:
  Docker containers are currently only supported for development and test environments, not for production. Use [Redis Enterprise on Kubernetes](/docs/latest/operate/kubernetes/) for a supported containerized deployment.

When deploying Redis Software using Docker for testing, several common topologies are available, according to your requirements:

[Single-node cluster](#single-node) â For local development or functional testing

[Multi-node cluster on a single host](#multi-node-one-host) â For a small-scale deployment that is similar to production

[Multi-node cluster with multiple hosts](#multi-node-multi-host) â For more predictable performance or high availability compared to single-host deployments

### Single node

The simplest topology is to run a single-node Redis Software cluster with a single container on a single host machine. You can use this topology for local development or functional testing.

Single-node clusters have limited functionality. For example, Redis Software can't use replication or protect against failures if the cluster has only one node.

[](/docs/latest/images/rs/RS-Docker-container.png)

### Multiple nodes on one host

You can create a multi-node Redis Software cluster by deploying multiple containers to a single host machine. The resulting cluster is scale minimized but similar to production deployments.

However, this will also have several limitations.  For example, you cannot map the same port on multiple containers on the same host.

[](/docs/latest/images/rs/RS-Docker-cluster-single-host.png)

### Multiple nodes and hosts

You can create a multi-node Redis Software cluster with multiple containers by deploying each container to a different host machine.

This topology minimizes interference between containers, allowing for the testing of more Redis Software features.

[](/docs/latest/images/rs/RS-Docker-cluster-multi-host.png)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page