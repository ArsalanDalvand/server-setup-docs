---
title: "Install Redis Software on Linux"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/install/install-on-linux/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/install/install-on-linux"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install Redis Software on Linux"
---
# Install Redis Software on Linux

Install Redis Software on Linux.

            Redis Software
          

After you [[operate/rs/installing-upgrading/install/prepare-install/download-install-package|download a Redis Software installation package]], install it on one of the nodes in the cluster.

For installation on machines without an internet connection, see [[operate/rs/installing-upgrading/install/offline-installation|Offline installation]].

## Install on Linux

To install Redis Software, use the command line:

Copy the installation package to the node.

On the node, change to the directory where the installation package is located and extract the installation files:

```sh
tar vxf <tarfile name>
```

(Optional) Use the [GPG key file](../GPG-KEY-redislabs-packages.gpg) to confirm the authenticity of Ubuntu/Debian or RHEL RPM packages:

For Ubuntu:

Import the key:

```sh
gpg --import <path to GPG key>
```

Verify the package signature:

```sh
dpkg-sig --verify </path-to/package.deb>
```

For RHEL:

Import the key:

```sh
rpm --import <path to GPG key>
```

Verify the package signature:

```sh
rpm --checksig </path-to/package.rpm>
```

To start the installation process, run the installation script. See [installation script options](/docs/latest/operate/rs/installing-upgrading/install/install-script/) for a complete list of command-line options.

For default installation:

```sh
sudo ./install.sh
```

For custom installation directories:

If you need to specify custom storage paths for persistent or ephemeral storage during cluster setup, you must install Redis Software to custom directories.

```sh
sudo ./install.sh --install-dir <path> --config-dir <path> --var-dir <path>
```

For detailed information about custom installations, see [Customize installation directories](/docs/latest/operate/rs/installing-upgrading/install/customize-install-directories/).

Note:

The Redis Software files are installed in the default [[operate/rs/installing-upgrading/install/plan-deployment/file-locations|file locations]] unless you specify custom directories.
By default, Redis Software runs on the OS as the `redislabs` user and `redislabs` group. If needed, you can [[operate/rs/installing-upgrading/install/customize-user-and-group|specify a different user and group]] during the installation.
You must either be the root user or use `sudo` to run the installation script.

Answer the [[operate/rs/installing-upgrading/install/manage-installation-questions|installation questions]] when shown to complete the installation process.

Note:

To skip the installation questions, use one of the following methods:

Run `./install.sh -y` to answer yes to all of the questions.
Create an [[operate/rs/installing-upgrading/install/manage-installation-questions#configure-file-to-answer|answer file]] to answer installation questions automatically.

When installation completes successfully, the output displays the Cluster Manager UI's IP address:

```sh
Summary:
-------
ALL TESTS PASSED.
2017-04-24 10:54:15 [!] Please logout and login again to make
sure all environment changes are applied.
2017-04-24 10:54:15 [!] Point your browser at the following
URL to continue:
2017-04-24 10:54:15 [!] https://<your_ip_here>:8443
```

Repeat this process for each node in the cluster.

## Redis Flex and Auto Tiering installation

If you want to use Redis Flex or Auto Tiering for your databases, review the prerequisites, storage requirements, and [other considerations](/docs/latest/operate/rs/databases/flash/) and prepare and format the flash memory.

After you install Redis Software, use the `prepare_flash` script to prepare and format flash memory:

```sh
sudo /opt/redislabs/sbin/prepare_flash.sh
```

This script finds unformatted disks and mounts them as RAID partitions in `/var/opt/redislabs/flash`.

To verify the disk configuration, run:

```sh
sudo lsblk
```

## More info and options

To learn more about customization and find answers to related questions, see:

[[operate/rs/installing-upgrading/configuring/centos-rhel-firewall|CentOS/RHEL firewall configuration]]
[Change socket file location](/docs/latest/operate/rs/installing-upgrading/configuring/change-location-socket-files/)
[[operate/rs/networking/cluster-dns|Cluster DNS configuration]]
[[operate/rs/networking/cluster-lba-setup|Cluster load balancer setup]]
[[operate/rs/networking/mdns|mDNS client prerequisites]]
[[operate/rs/installing-upgrading/install/plan-deployment/file-locations|File locations]]
[[operate/rs/installing-upgrading/install/plan-deployment/supported-platforms|Supported platforms]]

## Limitations

Several Redis Software installation reference files are installed to the directory `/etc/opt/redislabs/` even if you use [custom installation directories](/docs/latest/operate/rs/installing-upgrading/install/customize-install-directories/).

As a workaround to install Redis Software without using any root directories, do the following before installing Redis Software:

Create all custom, non-root directories you want to use with Redis Software.

Mount `/etc/opt/redislabs` to one of the custom, non-root directories.

## Next steps

[[operate/rs/clusters/new-cluster-setup|Create]]
or [join](/docs/latest/operate/rs/clusters/add-node/) an existing Redis Software cluster.

[Create a database](/docs/latest/operate/rs/databases/create/).

For geo-distributed Active-Active replication, create an [Active-Active](/docs/latest/operate/rs/databases/active-active/create/) database.

[[operate/rs/security/access-control/create-users|Add users]] to the cluster with specific permissions.  To begin, start with [[operate/rs/security/access-control|Access control]].

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page