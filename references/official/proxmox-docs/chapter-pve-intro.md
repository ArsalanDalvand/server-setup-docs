---
title: "Introduction"
source: "https://pve.proxmox.com/pve-docs/chapter-pve-intro.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "chapter-pve-intro"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "Introduction"
---
[â](#)

Proxmox VE is a platform for running virtual machines and containers. It is
based on Debian GNU/Linux and completely open source. For maximum
flexibility, Proxmox VE provides two virtualization technologiesâKernel-based
Virtual Machine (KVM) and Linux Containers (LXC). [](#chapter_introduction)

A core design goal is to make administration as easy as possible. You
can use Proxmox VE on a single node, or assemble a cluster of many nodes.
All management tasks can be done through the web-based management
interface, and even a novice user can set up and install Proxmox VE within
minutes.

## Central Management

While many people start with a single node, Proxmox VE can scale out to a
large set of clustered nodes. The cluster stack is fully integrated
and ships with the default installation.

Unique Multi-Master Design

Every node in a Proxmox VE cluster can perform all management tasks. There
is no single-point-of-failure central management serverâany node can
manage the entire cluster. This multi-master architecture keeps the
cluster manageable even if individual nodes are temporarily
unavailable.

Proxmox Cluster File System (pmxcfs)

Proxmox VE uses the Proxmox Cluster File System (pmxcfs), a
database-driven file system for storing configuration files. It can
store the configuration of thousands of virtual machines. Corosync
replicates these files in real time to all cluster nodes. The file
system stores all data inside a persistent database on disk, while a
copy resides in RAM, providing a maximum storage size of 30 MBâmore
than enough for thousands of VMs.

Proxmox VE is the only virtualization platform using this cluster file
system.

Web-based Management Interface

Proxmox VE is simple to use. The included web-based interface, built on
the ExtJS JavaScript framework, provides a clean overview of all KVM
guests and Linux containers, and full control over all cluster
resources. You can manage VMs, containers, storage, and networking
from the GUI, including history and syslog viewing for each node,
running backup or restore jobs, live migration, and HA-triggered
activities. There is no need to install a separate management tool or
additional management node.

Command Line

For advanced users who prefer the comfort of a Unix shell or Windows
PowerShell, Proxmox VE provides a command-line interface to manage all
components of the virtual environment. This command-line interface
has intelligent tab completion and full documentation in the form of
man pages.

REST API

Proxmox VE uses a RESTful API with JSON as the primary data format. The
entire API is formally defined using JSON Schema, enabling fast and
easy integration for third-party management tools such as custom
hosting environments.

Role-based Administration

You can define granular access for all objects (such as VMs, storage,
and nodes) by using role-based user and permission management. Each
permission specifies a subject (a user or group) and a role (set of
privileges) on a specific path, following the access control list
model.

Authentication Realms

Proxmox VE supports multiple authentication sources, including Microsoft
Active Directory, LDAP, Linux PAM standard authentication, and the
built-in Proxmox VE authentication server.

## Flexible Storage

The Proxmox VE storage model is very flexible. Virtual machine images can
be stored on one or several local storages, or on shared storage such
as NFS or SAN. You may configure as many storage definitions as you
like, using all storage technologies available for Debian GNU/Linux.

One major benefit of storing VMs on shared storage is the ability to
live-migrate running machines without any downtime, as all nodes in
the cluster have direct access to VM disk images.

The following network storage types are supported:

LVM group (network backing with iSCSI targets)

iSCSI target

iSCSI LUNs (direct use)

NFS share

CIFS share

Ceph RBD

CephFS

Proxmox Backup Server

The following local storage types are supported:

LVM group (local backing devices such as block devices, Fibre Channel
  devices, and DRBD)

Directory (storage on existing file system)

ZFS

## Integrated Backup and Restore

The integrated backup tool (vzdump) creates consistent snapshots of
running containers and KVM guests. It creates an archive of the VM or
container data, including the configuration files.

Additionally, the [[chapter-pvesm.html#storage_pbs|Proxmox Backup Server integration]]
provides advanced features, including the ability to combine full backups
and data deduplication, perform client-side encryption, back up to tape
or S3 object storage, and sync with other offsite Proxmox Backup Server
installations.

KVM live backup works for all storage types including VM images on NFS,
CIFS, iSCSI LUN, and Ceph RBD. The backup format is optimized for
storing VM backups efficientlyâusing sparse files, out-of-order data, and
minimized I/O.

## High Availability Cluster

A multi-node Proxmox VE HA cluster enables highly available virtual machines
and containers. The integrated
[[chapter-ha-manager.html#chapter_ha_manager|High Availability (HA) manager]] automatically
detects node failures using watchdog-based fencing and restarts affected
services on healthy nodes. No external tools like Pacemaker are neededâthe
HA stack ships with Proxmox VE and is configured entirely through the web
interface or command line.

## Flexible Networking

Proxmox VE uses a bridged networking model. All VMs can share one bridge
as if virtual network cables from each guest were all plugged into
the same switch. For connecting VMs to the outside world, bridges are
attached to physical network cards and assigned a TCP/IP
configuration.

For further flexibility, Virtual LANs (VLANs, IEEE 802.1q) and
network bonding/aggregation are available, allowing you to build
complex, flexible virtual networks for the Proxmox VE hosts and leverage
the full power of the Linux network stack.

For larger or multi-tenant environments, the
[[chapter-pvesdn.html#chapter_pvesdn|Software Defined Network (SDN)]] stack provides
zone-based network isolation and supports protocols such as VXLAN and
EVPN. SDN is fully integrated into the web interface and installed by
default.

## Integrated Firewall

The integrated firewall allows you to filter network packets on
any VM or container interface. Common sets of firewall rules can
be grouped into “security groups”.

## Hyper-converged Infrastructure

Proxmox VE is a virtualization platform that tightly integrates compute, storage and
networking resources, manages highly available clusters, backup/restore as well
as disaster recovery. All components are software-defined and compatible with
one another.

Therefore it is possible to administrate them like a single system via the
centralized web management interface. These capabilities make Proxmox VE an ideal
choice to deploy and manage an open source
[hyper-converged infrastructure](https://en.wikipedia.org/wiki/Hyper-converged_infrastructure).

### Benefits of a Hyper-Converged Infrastructure (HCI) with Proxmox VE

A hyper-converged infrastructure (HCI) is especially useful for deployments in
which a high infrastructure demand meets a low administration budget, for
distributed setups such as remote and branch office environments or for virtual
private and public clouds.

HCI provides the following advantages:

Scalability: seamless expansion of compute, network and storage devices (i.e.
  scale up servers and storage quickly and independently from each other).

Low cost: Proxmox VE is open source and integrates all components you need such as
  compute, storage, networking, backup, and management center. It can replace
  an expensive compute/storage infrastructure.

Data protection and efficiency: services such as backup and disaster recovery
  are integrated.

Simplicity: easy configuration and centralized administration.

Open Source: No vendor lock-in.

### Hyper-Converged Infrastructure: Storage

Proxmox VE has tightly integrated support for deploying a hyper-converged storage
infrastructure. You can, for example, deploy and manage the following two
storage technologies by using the web interface only:

Ceph: a self-healing and self-managing shared, reliable and highly
  scalable storage system. Check out
  [[chapter-pveceph.html#chapter_pveceph|how to manage Ceph services on Proxmox VE nodes]]

ZFS: a combined file system and logical volume manager with extensive
  protection against data corruption, various RAID modes, fast and cheap
  snapshots - among other features. Find out
  [[chapter-sysadmin.html#chapter_zfs|how to leverage the power of ZFS on Proxmox VE nodes]].

Besides above, Proxmox VE has support to integrate a wide range of
additional storage technologies. You can find out about them in the
[[chapter-pvesm.html#chapter_storage|Storage Manager chapter]].

## Why Open Source

Proxmox VE uses a Linux kernel and is based on the Debian GNU/Linux
distribution. The source code of Proxmox VE is released under the
[GNU Affero General Public
License, version 3](https://www.gnu.org/licenses/agpl-3.0.html). This means that you are free to inspect the
source code at any time or contribute to the project yourself.

Proxmox is committed to using open source software whenever possible.
Open source software guarantees full access to all functionalityâas
well as high security and reliability. Everybody should have the right
to access the source code of software to run it, build on it, or
submit changes back to the project. Everybody is encouraged to
contribute while Proxmox ensures the product always meets professional
quality criteria.

Open source software also helps to keep costs low and makes core
infrastructure independent from a single vendor.

## Your Benefits with Proxmox VE

Open source software

No vendor lock-in

Linux kernel

Fast installation and easy to use

Web-based management interface

REST API

Huge active community

Low administration costs and simple deployment

## Getting Help

### Proxmox VE Wiki

The primary source of information is the [Proxmox VE Wiki](https://pve.proxmox.com/wiki/). It combines the reference
documentation with user contributed content.

### Community Support Forum

Proxmox VE itself is fully open source, so we always encourage our users to discuss
and share their knowledge using the [Proxmox VE Community Forum](https://forum.proxmox.com/). The forum is moderated by the
Proxmox support team, and has a large user base from all around the world.
Needless to say, such a large forum is a great place to get information.

### Mailing Lists

This is a fast way to communicate with the Proxmox VE community via email.

Mailing list for users:
  [Proxmox VE User List](http://lists.proxmox.com/cgi-bin/mailman/listinfo/pve-user)

Proxmox VE is fully open source and contributions are welcome! The primary
communication channel for developers is the:

Mailing list for developers:
  [Proxmox VE development
  discussion](http://lists.proxmox.com/cgi-bin/mailman/listinfo/pve-devel)

### Commercial Support

Proxmox Server Solutions GmbH also offers enterprise support available as
[Proxmox VE Subscription Service Plans](https://proxmox.com/en/proxmox-virtual-environment/pricing).
All users with a subscription get access to the Proxmox VE
[[chapter-sysadmin.html#sysadmin_enterprise_repo|Enterprise Repository]], and—with a Basic, Standard
or Premium subscription—also to the Proxmox Customer Portal. The customer
portal provides help and support with guaranteed response times from the Proxmox VE
developers.

For volume discounts, or more information in general, please contact
[sales@proxmox.com](mailto:sales@proxmox.com).

### Bug Tracker

Proxmox runs a public bug tracker at [https://bugzilla.proxmox.com](https://bugzilla.proxmox.com). If an issue
appears, file your report there. An issue can be a bug as well as a request for
a new feature or enhancement. The bug tracker helps to keep track of the issue
and will send a notification once it has been solved.

## Project History

The project started in 2007, followed by a first stable version in 2008.
At the time, it used OpenVZ for containers and KVM for virtual machines.
The clustering features were limited, and the user interface was simple
(a server-generated web page).

The project quickly developed new features using the
[Corosync](https://corosync.github.io/corosync/) cluster stack, and the
introduction of the Proxmox Cluster File System (pmxcfs) was a big step
forward, because it completely hides the cluster complexity from the
user. Managing a cluster of 16 nodes is as simple as managing a single
node.

The introduction of the REST API, with a complete declarative
specification written in JSON Schema, enabled other people to integrate
Proxmox VE into their infrastructure and made it easy to provide additional
services.

The REST API also made it possible to replace the original user interface
with a modern client-side single-page application using JavaScript. The
old Java-based VNC console code was replaced with
[noVNC](https://novnc.com/info.html), so only a web browser is needed to
manage VMs.

The support for various storage types is another big achievement.
Notably, Proxmox VE was the first distribution to ship
[ZFS on Linux](https://zfsonlinux.org/) by default in 2014. Another
milestone was the ability to run and manage
[Ceph](https://ceph.com/) storage on the hypervisor nodes. Such setups are
extremely cost effective.

When the project started, Proxmox was among the first companies
providing commercial support for KVM. The KVM project itself continuously
evolved and is now a widely used hypervisor. New features arrive with
each release. The KVM live backup feature, developed by Proxmox, makes
it possible to create snapshot backups on any storage type.

The most notable change with version 4.0 was the move from OpenVZ to
[LXC](https://linuxcontainers.org/). Containers are now deeply integrated,
and they can use the same storage and network features as virtual
machines. At the same time, the easy-to-use
[[chapter-ha-manager.html#chapter_ha_manager|High Availability (HA) manager]] was introduced,
simplifying the configuration and management of highly available setups.

During the development of Proxmox VE 5, asynchronous
[[chapter-pvesr.html#chapter_pvesr|storage replication]] as well as automated
[[chapter-sysadmin.html#sysadmin_certificate_management|certificate management]] using
ACME/Let’s Encrypt were introduced, among many other features.

The [[chapter-pvesdn.html#chapter_pvesdn|Software Defined Network (SDN)]] stack was
developed in cooperation with the community. It was integrated into the
web interface as an experimental feature in version 6.2, simplifying the
management of sophisticated network configurations. Since version 8.1,
the SDN integration is fully supported and installed by default.

2020 marked the release of a new project, the
[Proxmox
Backup Server](https://www.proxmox.com/en/products/proxmox-backup-server/overview), written in the Rust programming language. Proxmox Backup
Server is deeply integrated with Proxmox VE and significantly improves backup
capabilities by implementing incremental backups, deduplication, and much
more.

Another new tool, the [Proxmox
Offline Mirror](https://pom.proxmox.com), was released in 2022, enabling subscriptions for systems
which have no connection to the public internet.

The highly requested dark theme for the web interface was introduced in
2023. Later that year, version 8.0 integrated access to the Ceph
enterprise repository. Access to the most stable Ceph repository is
included with any Proxmox VE subscription.

Automated and unattended installation for the official
[[chapter-pve-installation.html#installation_installer|ISO installer]] was introduced in version 8.2,
significantly simplifying large deployments of Proxmox VE.

With the [[chapter-qm.html#qm_import_virtual_machines|import wizard]], equally
introduced in version 8.2, users can easily and efficiently migrate
guests directly from other hypervisors like VMware ESXi
[Migrate to Proxmox VE
[https://pve.proxmox.com/wiki/Migrate_to_Proxmox_VE](https://pve.proxmox.com/wiki/Migrate_to_Proxmox_VE)].
Additionally, archives in Open Virtualization Format (OVF/OVA) can now be
directly imported from file-based storages in the web interface.

## Improving the Proxmox VE Documentation

Contributions and improvements to the Proxmox VE documentation are always welcome.
There are several ways to contribute.

If you find errors or other room for improvement in this documentation, please
file a bug at the [Proxmox bug tracker](https://bugzilla.proxmox.com/) to propose
a correction.

If you want to propose new content, choose one of the following options:

The wiki: For specific setups, how-to guides, or tutorials the wiki   is the
right option to contribute.

The reference documentation: For general content that will be   helpful to all
  users please propose your contribution for the   reference documentation. This
  includes all information about how to install, configure, use, and
  troubleshoot Proxmox VE features. The reference documentation is written in the
  [asciidoc format](https://en.wikipedia.org/wiki/AsciiDoc). To edit the
  documentation you need to clone the git repository at
  git://git.proxmox.com/git/pve-docs.git; then follow the
  [README.adoc](https://git.proxmox.com/?p=pve-docs.git;a=blob_plain;f=README.adoc;hb=HEAD)
  document.

If you are interested in working on the Proxmox VE codebase, the
[Developer Documentation](https://pve.proxmox.com/wiki/Developer_Documentation) wiki article will
show you where to start.

## Translating Proxmox VE

The Proxmox VE user interface is in English by default. However, thanks to the
contributions of the community, translations to other languages are also available.
We welcome any support in adding new languages, translating the latest features, and
improving incomplete or inconsistent translations.

We use [gettext](https://www.gnu.org/software/gettext/) for the management of the
translation files. Tools like [Poedit](https://poedit.net/) offer a nice user
interface to edit the translation files, but you can use whatever editor you’re
comfortable with. No programming knowledge is required for translating.

### Translating with git

The language files are available as a
[git repository](https://git.proxmox.com/?p=proxmox-i18n.git). If you are familiar
with git, please contribute according to our
[Developer Documentation](https://pve.proxmox.com/wiki/Developer_Documentation).

You can create a new translation by doing the following (replace <LANG> with the
language ID):

```bash
# git clone git://git.proxmox.com/git/proxmox-i18n.git
# cd proxmox-i18n
# make init-<LANG>.po
```

Or you can edit an existing translation, using the editor of your choice:

```bash
# poedit <LANG>.po
```

### Translating without git

Even if you are not familiar with git, you can help translate Proxmox VE.
To start, you can download the language files
[here](https://git.proxmox.com/?p=proxmox-i18n.git;a=tree). Find the
language you want to improve, then right click on the "raw" link of this language
file and select Save Link As…. Make your changes to the file, and then
send your final translation directly to office(at)proxmox.com, together with a
signed
[contributor license agreement](https://pve.proxmox.com/wiki/Developer_Documentation#Software_License_and_Copyright).

### Testing the Translation

In order for the translation to be used in Proxmox VE, you must first translate
the .po file into a .js file. You can do this by invoking the following script,
which is located in the same repository:

```bash
# ./po2js.pl -t pve xx.po >pve-lang-xx.js
```

The resulting file pve-lang-xx.js can then be copied to the directory
/usr/share/pve-i18n, on your proxmox server, in order to test it out.

Alternatively, you can build a deb package by running the following command from
the root of the repository:

```bash
# make deb
```

For either of these methods to work, you need to have the following
perl packages installed on your system. For Debian/Ubuntu:

```bash
# apt-get install perl liblocale-po-perl libjson-perl
```

### Sending the Translation

You can send the finished translation (.po file) to the Proxmox team at the address
office(at)proxmox.com, along with a signed contributor license agreement.
Alternatively, if you have some developer experience, you can send it as a
patch to the Proxmox VE development mailing list. See
[Developer Documentation](https://pve.proxmox.com/wiki/Developer_Documentation).