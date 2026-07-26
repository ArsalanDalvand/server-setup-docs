---
title: "pvenode(1)"
source: "https://pve.proxmox.com/pve-docs/pvenode.1.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "pvenode.1"
category: "proxmox"
retrieved_at: "2026-07-26T15:55:48+00:00"
tags:
  - proxmox
  - hypervisor
  - lxc-virtualization
  - official-documentation
aliases:
  - "pvenode(1)"
---
[â](#)

## NAME

pvenode - Proxmox VE Node Management

## SYNOPSIS

pvenode <COMMAND> [ARGS] [OPTIONS] [](#cli_pvenode)

pvenode acme account deactivate [<name>] [](#cli_pvenode_acme_account_deactivate)

Deactivate existing ACME account at CA.

<name>: <name> (default = default)

ACME account config file name.

pvenode acme account info [<name>] [FORMAT_OPTIONS] [](#cli_pvenode_acme_account_info)

Return existing ACME account information.

<name>: <name> (default = default)

ACME account config file name.

pvenode acme account list [](#cli_pvenode_acme_account_list)

ACMEAccount index.

pvenode acme account register [<name>] {<contact>} [OPTIONS] [](#cli_pvenode_acme_account_register)

Register a new ACME account with a compatible CA.

<name>: <name> (default = default)

ACME account config file name.

<contact>: <string>

Contact email addresses.

--directory ^https?://.*

URL of ACME CA directory endpoint.

pvenode acme account update [<name>] [OPTIONS] [](#cli_pvenode_acme_account_update)

Update existing ACME account information with CA. Note: not specifying any
new account information triggers a refresh.

<name>: <name> (default = default)

ACME account config file name.

--contact <string>

Contact email addresses.

pvenode acme cert order [OPTIONS] [](#cli_pvenode_acme_cert_order)

Order a new certificate from ACME-compatible CA.

--force <boolean> (default = 0)

Overwrite existing custom certificate.

pvenode acme cert renew [OPTIONS] [](#cli_pvenode_acme_cert_renew)

Renew existing certificate from CA.

--force <boolean> (default = 0)

Force renewal even if expiry is more than 30 days away.

pvenode acme cert revoke [](#cli_pvenode_acme_cert_revoke)

Revoke existing certificate from CA.

pvenode acme plugin add <type> <id> [OPTIONS] [](#cli_pvenode_acme_plugin_add)

Add ACME plugin configuration.

<type>: <dns | standalone>

ACME challenge type.

<id>: <string>

ACME Plugin ID name

--api <1984hosting | acmedns | acmeproxy | active24 | ad | ali | alviy | anx | artfiles | arvan | aurora | autodns | aws | azion | azure | beget | bookmyname | bunny | cf | clouddns | cloudns | cn | conoha | constellix | cpanel | curanet | cyon | da | ddnss | desec | df | dgon | dnsexit | dnshome | dnsimple | dnsservices | doapi | domeneshop | dp | dpi | dreamhost | duckdns | durabledns | dyn | dynu | dynv6 | easydns | edgecenter | edgedns | euserv | exoscale | fornex | freedns | freemyip | gandi_livedns | gcloud | gcore | gd | geoscaling | googledomains | he | he_ddns | hetzner | hetznercloud | hexonet | hostingde | huaweicloud | infoblox | infomaniak | internetbs | inwx | ionos | ionos_cloud | ipv64 | ispconfig | jd | joker | kappernet | kas | kinghost | knot | la | leaseweb | lexicon | limacity | linode | linode_v4 | loopia | lua | maradns | me | miab | mijnhost | misaka | myapi | mydevil | mydnsjp | mythic_beasts | namecheap | namecom | namesilo | nanelo | nederhost | neodigit | netcup | netlify | nic | njalla | nm | nsd | nsone | nsupdate | nw | oci | omglol | one | online | openprovider | openprovider_rest | openstack | opnsense | ovh | pdns | pleskxml | pointhq | porkbun | rackcorp | rackspace | rage4 | rcode0 | regru | scaleway | schlundtech | selectel | selfhost | servercow | simply | spaceship | technitium | tele3 | tencent | timeweb | transip | udr | ultra | unoeuro | variomedia | veesp | vercel | vscale | vultr | websupport | west_cn | world4you | yandex360 | yc | zilore | zone | zoneedit | zonomi>

API plugin name

--data File with one key-value pair per line, will be base64url encode for storage in plugin config.

DNS plugin data. (base64 encoded)

--disable <boolean>

Flag to disable the config.

--nodes <string>

List of cluster node names.

--validation-delay <integer> (0 - 172800) (default = 30)

Extra delay in seconds to wait before requesting validation. Allows to cope with a long TTL of DNS records.

pvenode acme plugin config <id> [FORMAT_OPTIONS] [](#cli_pvenode_acme_plugin_config)

Get ACME plugin configuration.

<id>: <string>

Unique identifier for ACME plugin instance.

pvenode acme plugin list [OPTIONS] [FORMAT_OPTIONS] [](#cli_pvenode_acme_plugin_list)

ACME plugin index.

--type <dns | standalone>

Only list ACME plugins of a specific type

pvenode acme plugin remove <id> [](#cli_pvenode_acme_plugin_remove)

Delete ACME plugin configuration.

<id>: <string>

Unique identifier for ACME plugin instance.

pvenode acme plugin set <id> [OPTIONS] [](#cli_pvenode_acme_plugin_set)

Update ACME plugin configuration.

<id>: <string>

ACME Plugin ID name

--api <1984hosting | acmedns | acmeproxy | active24 | ad | ali | alviy | anx | artfiles | arvan | aurora | autodns | aws | azion | azure | beget | bookmyname | bunny | cf | clouddns | cloudns | cn | conoha | constellix | cpanel | curanet | cyon | da | ddnss | desec | df | dgon | dnsexit | dnshome | dnsimple | dnsservices | doapi | domeneshop | dp | dpi | dreamhost | duckdns | durabledns | dyn | dynu | dynv6 | easydns | edgecenter | edgedns | euserv | exoscale | fornex | freedns | freemyip | gandi_livedns | gcloud | gcore | gd | geoscaling | googledomains | he | he_ddns | hetzner | hetznercloud | hexonet | hostingde | huaweicloud | infoblox | infomaniak | internetbs | inwx | ionos | ionos_cloud | ipv64 | ispconfig | jd | joker | kappernet | kas | kinghost | knot | la | leaseweb | lexicon | limacity | linode | linode_v4 | loopia | lua | maradns | me | miab | mijnhost | misaka | myapi | mydevil | mydnsjp | mythic_beasts | namecheap | namecom | namesilo | nanelo | nederhost | neodigit | netcup | netlify | nic | njalla | nm | nsd | nsone | nsupdate | nw | oci | omglol | one | online | openprovider | openprovider_rest | openstack | opnsense | ovh | pdns | pleskxml | pointhq | porkbun | rackcorp | rackspace | rage4 | rcode0 | regru | scaleway | schlundtech | selectel | selfhost | servercow | simply | spaceship | technitium | tele3 | tencent | timeweb | transip | udr | ultra | unoeuro | variomedia | veesp | vercel | vscale | vultr | websupport | west_cn | world4you | yandex360 | yc | zilore | zone | zoneedit | zonomi>

API plugin name

--data File with one key-value pair per line, will be base64url encode for storage in plugin config.

DNS plugin data. (base64 encoded)

--delete <string>

A list of settings you want to delete.

--digest <string>

Prevent changes if current configuration file has a different digest. This can be used to prevent concurrent modifications.

--disable <boolean>

Flag to disable the config.

--nodes <string>

List of cluster node names.

--validation-delay <integer> (0 - 172800) (default = 30)

Extra delay in seconds to wait before requesting validation. Allows to cope with a long TTL of DNS records.

pvenode cert delete [<restart>] [](#cli_pvenode_cert_delete)

DELETE custom certificate chain and key.

<restart>: <boolean> (default = 0)

Restart pveproxy.

pvenode cert info [FORMAT_OPTIONS] [](#cli_pvenode_cert_info)

Get information about node’s certificates.

pvenode cert set <certificates> [<key>] [OPTIONS] [FORMAT_OPTIONS] [](#cli_pvenode_cert_set)

Upload or update custom certificate chain and key.

<certificates>: <string>

PEM encoded certificate (chain).

<key>: <string>

PEM encoded private key.

--force <boolean> (default = 0)

Overwrite existing custom or ACME certificate files.

--restart <boolean> (default = 0)

Restart pveproxy.

pvenode config get [OPTIONS] [](#cli_pvenode_config_get)

Get node configuration options.

--property <acme | acmedomain0 | acmedomain1 | acmedomain2 | acmedomain3 | acmedomain4 | acmedomain5 | ballooning-target | description | location | startall-onboot-delay | wakeonlan> (default = all)

Return only a specific property from the node configuration.

pvenode config set [OPTIONS] [](#cli_pvenode_config_set)

Set node configuration options.

--acme [account=<name>] [,domains=<domain[;domain;...]>]

Node specific ACME settings.

--acmedomain[n] [domain=]<domain> [,alias=<domain>] [,plugin=<name of the plugin configuration>]

ACME domain and validation plugin

--ballooning-target <integer> (0 - 100) (default = 80)

RAM usage target for ballooning (in percent of total memory)

--delete <string>

A list of settings you want to delete.

--description <string>

Description for the Node. Shown in the web-interface node notes panel. This is saved as comment inside the configuration file.

--digest <string>

Prevent changes if current configuration file has different SHA1 digest. This can be used to prevent concurrent modifications.

--location latitude=<number> ,longitude=<number> [,name=<name>]

The location of the node. Overrides the default from the datacenter config.

--startall-onboot-delay <integer> (0 - 300) (default = 0)

Initial delay in seconds, before starting all the Virtual Guests with on-boot enabled.

--wakeonlan [mac=]<MAC address> [,bind-interface=<bind interface>] [,broadcast-address=<IPv4 broadcast address>]

Node specific wake on LAN settings.

pvenode help [OPTIONS] [](#cli_pvenode_help)

Get help about specified command.

--extra-args <array>

Shows help for a specific command

--verbose <boolean>

Verbose output format.

pvenode migrateall <target> [OPTIONS] [](#cli_pvenode_migrateall)

Migrate all VMs and Containers.

<target>: <string>

Target node.

--max-workers <integer> (1 - 64)

Maximal number of parallel migration job. If not set, uses’max_workers' from datacenter.cfg. One of both must be set!

--maxworkers <integer> (1 - 64)

Maximal number of parallel migration job. If not set, uses’max_workers' from datacenter.cfg. One of both must be set!Deprecated, use max-workers instead.

--vms <string>

Only consider Guests with these IDs.

--with-local-disks <boolean>

Enable live storage migration for local disk

pvenode startall [OPTIONS] [](#cli_pvenode_startall)

Start all VMs and containers located on this node (by default only those
with onboot=1).

--force <boolean> (default = off)

Issue start command even if virtual guest have onboot not set or set to off.

--max-workers <integer> (1 - 64)

Defines the maximum number of tasks running concurrently. If not set, uses max_workers from datacenter.cfg, and if that’s not set, the available CPU threads, clamped to a maximum of 8, are used.

--vms <string>

Only consider guests from this comma separated list of VMIDs.

pvenode stopall [OPTIONS] [](#cli_pvenode_stopall)

Stop all VMs and Containers.

--force-stop <boolean> (default = 1)

Force a hard-stop after the timeout.

--max-workers <integer> (1 - 64)

Defines the maximum number of tasks running concurrently. If  not set, uses max_workers from datacenter.cfg, and if that’s not set, the available CPU threads, clamped to a maximum of 8, are used.

--timeout <integer> (0 - 7200) (default = 180)

Timeout for each guest shutdown task. Depending on force-stop, the shutdown gets then simply aborted or a hard-stop is forced.

--vms <string>

Only consider Guests with these IDs.

pvenode task list [OPTIONS] [FORMAT_OPTIONS] [](#cli_pvenode_task_list)

Read task list for one node (finished tasks).

--errors <boolean> (default = 0)

Only list tasks with a status of ERROR.

--limit <integer> (0 - N) (default = 50)

Only list this number of tasks.

--since <integer>

Only list tasks since this UNIX epoch.

--source <active | all | archive> (default = archive)

List archived, active or all tasks.

--start <integer> (0 - N) (default = 0)

List tasks beginning from this offset.

--statusfilter <string>

List of Task States that should be returned.

--typefilter <string>

Only list tasks of this type (e.g., vzstart, vzdump).

--until <integer>

Only list tasks until this UNIX epoch.

--userfilter <string>

Only list tasks from this user.

--vmid <integer> (100 - 999999999)

Only list tasks for this VM.

pvenode task log <upid> [OPTIONS] [](#cli_pvenode_task_log)

Read task log.

<upid>: <string>

The task’s unique ID.

--download <boolean>

Whether the tasklog file should be downloaded. This parameter can’t be used in conjunction with other parameters

--start <integer> (0 - N) (default = 0)

Start at this line when reading the tasklog

pvenode task status <upid> [FORMAT_OPTIONS] [](#cli_pvenode_task_status)

Read task status.

<upid>: <string>

The task’s unique ID.

pvenode wakeonlan <node> [](#cli_pvenode_wakeonlan)

Try to wake a node via wake on LAN network packet.

<node>: <string>

target node for wake on LAN packet

## DESCRIPTION

The Proxmox VE node management tool (pvenode) allows you to control node specific
settings and resources.

Currently pvenode allows you to set a node’s description, run various
bulk operations on the node’s guests, view the node’s task history, and
manage the node’s SSL certificates, which are used for the API and the web GUI
through pveproxy.

## FORMAT_OPTIONS

It is possible to specify the output format using the
--output-format parameter. The default format text uses ASCII-art
to draw nice borders around tables. It additionally transforms some
values into human-readable text, for example:

Unix epoch is displayed as ISO 8601 date string.

Durations are displayed as week/day/hour/minute/second count, i.e 1d 5h.

Byte sizes value include units (B, KiB, MiB, GiB, TiB, PiB).

Fractions are display as percentage, i.e. 1.0 is displayed as 100%.

You can also completely suppress output using option --quiet.

--human-readable <boolean> (default = 1)

Call output rendering functions to produce human readable text.

--noborder <boolean> (default = 0)

Do not draw borders (for text format).

--noheader <boolean> (default = 0)

Do not show column headers (for text format).

--output-format <json | json-pretty | text | yaml> (default = text)

Output format.

--quiet <boolean>

Suppress printing results.

### Examples

Install an externally provided certificate

pvenode cert set certificate.crt certificate.key -force

Both files need to be PEM encoded. certificate.key contains the private key
and certificate.crt contains the whole certificate chain.

Setup ACME account and order a certificate for the local node.

```bash
pvenode acme account register default mail@example.invalid
pvenode config set --acme domains=example.invalid
pvenode acme cert order
systemctl restart pveproxy
```

### Wake-on-LAN

Wake-on-LAN (WoL) allows you to switch on a sleeping computer in the network, by
sending a magic packet. At least one NIC must support this feature, and the
respective option needs to be enabled in the computer’s firmware (BIOS/UEFI)
configuration. The option name can vary from Enable Wake-on-Lan to
Power On By PCIE Device; check your motherboard’s vendor manual, if you’re
unsure. ethtool can be used to check the WoL configuration of <interface>
by running:

```bash
ethtool <interface> | grep Wake-on
```

pvenode allows you to wake sleeping members of a cluster via WoL, using the
command:

```bash
pvenode wakeonlan <node>
```

This broadcasts the WoL magic packet on UDP port 9, containing the MAC address
of <node> obtained from the wakeonlan property. The node-specific
wakeonlan property can be set using the following command:

```bash
pvenode config set -wakeonlan XX:XX:XX:XX:XX:XX
```

The interface via which to send the WoL packet is determined from the default
route. It can be overwritten by setting the bind-interface via the following
command:

```bash
pvenode config set -wakeonlan XX:XX:XX:XX:XX:XX,bind-interface=<iface-name>
```

The broadcast address (default 255.255.255.255) used when sending the WoL
packet can further be changed by setting the broadcast-address explicitly
using the following command:

```bash
pvenode config set -wakeonlan XX:XX:XX:XX:XX:XX,broadcast-address=<broadcast-address>
```

### Task History

When troubleshooting server issues, for example, failed backup jobs, it can
often be helpful to have a log of the previously run tasks. With Proxmox VE, you can
access the nodes’s task history through the pvenode task command.

You can get a filtered list of a node’s finished tasks with the list
subcommand. For example, to get a list of tasks related to VM 100
that ended with an error, the command would be:

```bash
pvenode task list --errors --vmid 100
```

The log of a task can then be printed using its UPID:

```bash
pvenode task log UPID:pve1:00010D94:001CA6EA:6124E1B9:vzdump:100:root@pam:
```

### Bulk Guest Power Management

In case you have many VMs/containers, starting and stopping guests can be
carried out in bulk operations with the startall and stopall subcommands of
pvenode.  By default, pvenode startall will only start VMs/containers which
have been set to automatically start on boot (see
[[qm.1.html#qm_startup_and_shutdown|Automatic Start and Shutdown of Virtual Machines]]),
however, you can override this behavior with the --force flag. Both commands
also have a --vms option, which limits the stopped/started guests to the
specified VMIDs.

For example, to start VMs 100, 101, and 102, regardless of whether they
have onboot set, you can use:

```bash
pvenode startall --vms 100,101,102 --force
```

To stop these guests (and any other guests that may be running), use the
command:

```bash
pvenode stopall
```

The stopall command first attempts to perform a clean shutdown and then
waits until either all guests have successfully shut down or an overridable
timeout (3 minutes by default) has expired. Once that happens and the
force-stop parameter is not explicitly set to 0 (false), all virtual guests
that are still running are hard stopped.

### First Guest Boot Delay

In case your VMs/containers rely on slow-to-start external resources, for
example an NFS server, you can also set a per-node delay between the time Proxmox VE
boots and the time the first VM/container that is configured to autostart boots
(see [[qm.1.html#qm_startup_and_shutdown|Automatic Start and Shutdown of Virtual Machines]]).

You can achieve this by setting the following (where 10 represents the delay
in seconds):

```bash
pvenode config set --startall-onboot-delay 10
```

### Bulk Guest Migration

In case an upgrade situation requires you to migrate all of your guests from one
node to another, pvenode also offers the migrateall subcommand for bulk
migration. By default, this command will migrate every guest on the system to
the target node. It can however be set to only migrate a set of guests.

For example, to migrate VMs 100, 101, and 102, to the node pve2, with
live-migration for local disks enabled, you can run:

```bash
pvenode migrateall pve2 --vms 100,101,102 --with-local-disks
```

### RAM Usage Target for Ballooning

The target percentage for [[qm.1.html#qm_ballooning|automatic memory allocation]]
defaults to 80%. You can customize this target per node by setting the
ballooning-target property. For example, to target 90% host memory usage
instead:

```bash
pvenode config set --ballooning-target 90
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