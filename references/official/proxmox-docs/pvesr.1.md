---
title: "pvesr(1)"
source: "https://pve.proxmox.com/pve-docs/pvesr.1.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "pvesr.1"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "pvesr(1)"
---
[â](#)

## NAME

pvesr - Proxmox VE Storage Replication

## SYNOPSIS

pvesr <COMMAND> [ARGS] [OPTIONS] [](#cli_pvesr)

pvesr create-local-job <id> <target> [OPTIONS] [](#cli_pvesr_create-local-job)

Create a new replication job

<id>: [1-9][0-9]{2,8}-\d{1,9}

Replication Job ID. The ID is composed of a Guest ID and a job number, separated by a hyphen, i.e. <GUEST>-<JOBNUM>.

<target>: <string>

Target node.

--comment <string>

Description.

--disable <boolean>

Flag to disable/deactivate the entry.

--rate <number> (1 - N)

Rate limit in mbps (megabytes per second) as floating point number.

--remove_job <full | local>

Mark the replication job for removal. The job will remove all local replication snapshots. When set to full, it also tries to remove replicated volumes on the target. The job then removes itself from the configuration file.

--schedule <string> (default = */15)

Storage replication schedule. The format is a subset of systemd calendar events.

--source <string>

For internal use, to detect if the guest was stolen.

pvesr delete <id> [OPTIONS] [](#cli_pvesr_delete)

Mark replication job for removal.

<id>: [1-9][0-9]{2,8}-\d{1,9}

Replication Job ID. The ID is composed of a Guest ID and a job number, separated by a hyphen, i.e. <GUEST>-<JOBNUM>.

--force <boolean> (default = 0)

Will remove the jobconfig entry, but will not cleanup.

--keep <boolean> (default = 0)

Keep replicated data at target (do not remove).

pvesr disable <id> [](#cli_pvesr_disable)

Disable a replication job.

<id>: [1-9][0-9]{2,8}-\d{1,9}

Replication Job ID. The ID is composed of a Guest ID and a job number, separated by a hyphen, i.e. <GUEST>-<JOBNUM>.

pvesr enable <id> [](#cli_pvesr_enable)

Enable a replication job.

<id>: [1-9][0-9]{2,8}-\d{1,9}

Replication Job ID. The ID is composed of a Guest ID and a job number, separated by a hyphen, i.e. <GUEST>-<JOBNUM>.

pvesr finalize-local-job <id> [<extra-args>] [OPTIONS] [](#cli_pvesr_finalize-local-job)

Finalize a replication job. This removes all replications snapshots with
timestamps different than <last_sync>.

<id>: [1-9][0-9]{2,8}-\d{1,9}

Replication Job ID. The ID is composed of a Guest ID and a job number, separated by a hyphen, i.e. <GUEST>-<JOBNUM>.

<extra-args>: <array>

The list of volume IDs to consider.

--last_sync <integer> (0 - N)

Time (UNIX epoch) of last successful sync. If not specified, all replication snapshots gets removed.

pvesr help [OPTIONS] [](#cli_pvesr_help)

Get help about specified command.

--extra-args <array>

Shows help for a specific command

--verbose <boolean>

Verbose output format.

pvesr list [](#cli_pvesr_list)

List replication jobs.

pvesr prepare-local-job <id> [<extra-args>] [OPTIONS] [](#cli_pvesr_prepare-local-job)

Prepare for starting a replication job. This is called on the target node
before replication starts. This call is for internal use, and return a JSON
object on stdout. The method first test if VM <vmid> reside on the local
node. If so, stop immediately. After that the method scans all volume IDs
for snapshots, and removes all replications snapshots with timestamps
different than <last_sync>. It also removes any unused volumes. Returns a
hash with boolean markers for all volumes with existing replication
snapshots.

<id>: [1-9][0-9]{2,8}-\d{1,9}

Replication Job ID. The ID is composed of a Guest ID and a job number, separated by a hyphen, i.e. <GUEST>-<JOBNUM>.

<extra-args>: <array>

The list of volume IDs to consider.

--force <boolean> (default = 0)

Allow to remove all existion volumes (empty volume list).

--last_sync <integer> (0 - N)

Time (UNIX epoch) of last successful sync. If not specified, all replication snapshots get removed.

--parent_snapname <string>

The name of the snapshot.

--scan <string>

List of storage IDs to scan for stale volumes.

pvesr read <id> [](#cli_pvesr_read)

Read replication job configuration.

<id>: [1-9][0-9]{2,8}-\d{1,9}

Replication Job ID. The ID is composed of a Guest ID and a job number, separated by a hyphen, i.e. <GUEST>-<JOBNUM>.

pvesr run [OPTIONS] [](#cli_pvesr_run)

This method is called by the systemd-timer and executes all (or a specific)
sync jobs.

--id [1-9][0-9]{2,8}-\d{1,9}

Replication Job ID. The ID is composed of a Guest ID and a job number, separated by a hyphen, i.e. <GUEST>-<JOBNUM>.

--mail <boolean> (default = 0)

Send an email notification in case of a failure.

--verbose <boolean> (default = 0)

Print more verbose logs to stdout.

pvesr schedule-now <id> [](#cli_pvesr_schedule-now)

Schedule replication job to start as soon as possible.

<id>: [1-9][0-9]{2,8}-\d{1,9}

Replication Job ID. The ID is composed of a Guest ID and a job number, separated by a hyphen, i.e. <GUEST>-<JOBNUM>.

pvesr set-state <vmid> <state> [](#cli_pvesr_set-state)

Set the job replication state on migration. This call is for internal use.
It will accept the job state as ja JSON obj.

<vmid>: <integer> (100 - 999999999)

The (unique) ID of the VM.

<state>: <string>

Job state as JSON decoded string.

pvesr status [OPTIONS] [](#cli_pvesr_status)

List status of all replication jobs on this node.

--guest <integer> (100 - 999999999)

Only list replication jobs for this guest.

pvesr update <id> [OPTIONS] [](#cli_pvesr_update)

Update replication job configuration.

<id>: [1-9][0-9]{2,8}-\d{1,9}

Replication Job ID. The ID is composed of a Guest ID and a job number, separated by a hyphen, i.e. <GUEST>-<JOBNUM>.

--comment <string>

Description.

--delete <string>

A list of settings you want to delete.

--digest <string>

Prevent changes if current configuration file has a different digest. This can be used to prevent concurrent modifications.

--disable <boolean>

Flag to disable/deactivate the entry.

--rate <number> (1 - N)

Rate limit in mbps (megabytes per second) as floating point number.

--remove_job <full | local>

Mark the replication job for removal. The job will remove all local replication snapshots. When set to full, it also tries to remove replicated volumes on the target. The job then removes itself from the configuration file.

--schedule <string> (default = */15)

Storage replication schedule. The format is a subset of systemd calendar events.

--source <string>

For internal use, to detect if the guest was stolen.

## DESCRIPTION

The pvesr command-line tool manages the Proxmox VE storage replication
framework. Storage replication brings redundancy for guests using
local storage and reduces migration time.

It replicates guest volumes to another node so that all data is available
without using shared storage. Replication uses snapshots to minimize traffic
sent over the network. Therefore, new data is sent only incrementally after
the initial full sync. In the case of a node failure, your guest data is
still available on the replicated node.

The replication is done automatically in configurable intervals.
The minimum replication interval is one minute, and the maximal interval
once a week. The format used to specify those intervals is a subset of
systemd calendar events, see
[[pvesr.1.html#pvesr_schedule_time_format|Schedule Format]] section:

It is possible to replicate a guest to multiple target nodes,
but not twice to the same target node.

Each replications bandwidth can be limited, to avoid overloading a storage
or server.

Only changes since the last replication (so-called deltas) need to be
transferred if the guest is migrated to a node to which it already is
replicated. This reduces the time needed significantly. The replication
direction automatically switches if you migrate a guest to the replication
target node.

For example: VM100 is currently on nodeA and gets replicated to nodeB.
You migrate it to nodeB, so now it gets automatically replicated back from
nodeB to nodeA.

If you migrate to a node where the guest is not replicated, the whole disk
data must send over. After the migration, the replication job continues to
replicate this guest to the configured nodes.

High-Availability is allowed in combination with storage replication, but there
may be some data loss between the last synced time and the time a node failed.

## Supported Storage Types

Table 1. Storage Types

Description    
Plugin type 
Snapshots
Stable

ZFS (local)

zfspool

yes

yes

## Schedule Format

Replication uses calendar events for
configuring the schedule.

## Error Handling

If a replication job encounters problems, it is placed in an error state.
In this state, the configured replication intervals get suspended
temporarily. The failed replication is repeatedly tried again in a
30 minute interval.
Once this succeeds, the original schedule gets activated again.

### Possible issues

Some of the most common issues are in the following list. Depending on your
setup there may be another cause.

Network is not working.

No free space left on the replication target storage.

Storage with the same storage ID is not available on the target node.

You can always use the replication log to find out what is causing the problem.

### Migrating a guest in case of Error

In the case of a grave error, a virtual guest may get stuck on a failed
node. You then need to move it manually to a working node again.

### Example

Let’s assume that you have two guests (VM 100 and CT 200) running on node A
and replicate to node B.
Node A failed and can not get back online. Now you have to migrate the guest
to Node B manually.

connect to node B over ssh or open its shell via the web UI

check if that the cluster is quorate

```bash
# pvecm status
```

If you have no quorum, we strongly advise to fix this first and make the
  node operable again. Only if this is not possible at the moment, you may
  use the following command to enforce quorum on the current node:

```bash
# pvecm expected 1
```

Avoid changes which affect the cluster if expected votes are set
(for example adding/removing nodes, storages, virtual guests) at all costs.
Only use it to get vital guests up and running again or to resolve the quorum
issue itself.

move both guest configuration files form the origin node A to node B:

```bash
# mv /etc/pve/nodes/A/qemu-server/100.conf /etc/pve/nodes/B/qemu-server/100.conf
# mv /etc/pve/nodes/A/lxc/200.conf /etc/pve/nodes/B/lxc/200.conf
```

Now you can start the guests again:

```bash
# qm start 100
# pct start 200
```

Remember to replace the VMIDs and node names with your respective values.

## Managing Jobs

[](images/screenshot/gui-qemu-add-replication-job.png)

You can use the web GUI to create, modify, and remove replication jobs
easily. Additionally, the command-line interface (CLI) tool pvesr can be
used to do this.

You can find the replication panel on all levels (datacenter, node, virtual
guest) in the web GUI. They differ in which jobs get shown:
all, node- or guest-specific jobs.

When adding a new job, you need to specify the guest if not already selected
as well as the target node. The replication
[[pvesr.1.html#pvesr_schedule_time_format|schedule]] can be set if the default of all
15 minutes is not desired. You may impose a rate-limit on a replication
job. The rate limit can help to keep the load on the storage acceptable.

A replication job is identified by a cluster-wide unique ID. This ID is
composed of the VMID in addition to a job number.
This ID must only be specified manually if the CLI tool is used.

## Network

By default, replication traffic will go through the same network as the guest
migration traffic. If the migration network is not set, this will in turn
default to the management network.

To use a different network for the replication traffic, configure the
Replication Network in the web interface under Datacenter -> Options ->
Replication Settings or set the replication property in the datacenter.cfg
file:

```bash
# use dedicated replication network
replication: secure,network=10.1.2.0/24
```

## Command-line Interface Examples

Create a replication job which runs every 5 minutes with a limited bandwidth
of 10 Mbps (megabytes per second) for the guest with ID 100.

```bash
# pvesr create-local-job 100-0 pve1 --schedule "*/5" --rate 10
```

Disable an active job with ID 100-0.

```bash
# pvesr disable 100-0
```

Enable a deactivated job with ID 100-0.

```bash
# pvesr enable 100-0
```

Change the schedule interval of the job with ID 100-0 to once per hour.

```bash
# pvesr update 100-0 --schedule '*/00'
```

## Copyright and Disclaimer

Copyright © 2007-2022 Proxmox Server Solutions GmbH

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as
published by the Free Software Foundation, either version 3 of the
License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but
WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public
License along with this program.  If not, see
[https://www.gnu.org/licenses/](https://www.gnu.org/licenses/)