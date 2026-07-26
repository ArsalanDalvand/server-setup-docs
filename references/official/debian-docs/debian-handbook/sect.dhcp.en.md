---
title: "10.8.2. DHCP and DNS"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.dhcp.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.dhcp.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "10.8.2. DHCP and DNS"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.domain-name-servers.en.md)The Debian Administrator's Handbook[Next](sect.network-diagnosis-tools.en.md)

## 10.8.Â DHCP

			DHCP (for Dynamic Host Configuration Protocol) is a protocol by which a machine can automatically get its network configuration when it boots. This allows centralizing the management of network configurations, and ensuring that all desktop machines get similar settings.
		
			A DHCP server provides many network-related parameters. The most common of these is an IP address and the network where the machine belongs, but it can also provide other information, such as DNS servers, WINS servers, NTP servers, and so on.
		
			The Internet Software Consortium (also involved in developing bind) is the main author of the DHCP server. The matching Debian package is isc-dhcp-server.
		

### 10.8.1.Â Configuring

				The first elements that need to be edited in the DHCP server configuration files (/etc/dhcp/dhcpd.conf, and /etc/dhcp/dhcpd6.conf for IPv6) are the domain name and the DNS servers. If this server is alone on the local network (as defined by the broadcast propagation), the authoritative directive must also be enabled (or uncommented). One also needs to create a subnet section describing the local network and the configuration information to be provided. The following example fits a 192.168.0.0/24 local network with a router at 192.168.0.1 serving as the gateway. Available IP addresses are in the range 192.168.0.128 to 192.168.0.254.
			

ExampleÂ 10.15.Â Excerpt of /etc/dhcp/dhcpd.conf

```bash
#
# Sample configuration file for ISC dhcpd for Debian
#

# The ddns-updates-style parameter controls whether or not the server will
# attempt to do a DNS update when a lease is confirmed. We default to the
# behavior of the version 2 packages ('none', since DHCP v2 didn't
# have support for DDNS.)
ddns-update-style interim;

# option definitions common to all supported networks...
option domain-name "internal.falcot.com";
option domain-name-servers ns.internal.falcot.com;

default-lease-time 600;
max-lease-time 7200;

# If this DHCP server is the official DHCP server for the local
# network, the authoritative directive should be uncommented.
authoritative;

# Use this to send dhcp log messages to a different log file (you also
# have to hack syslog.conf to complete the redirection).
log-facility local7;

# My subnet
subnet 192.168.0.0 netmask 255.255.255.0 {
    option routers 192.168.0.1;
    option broadcast-address 192.168.0.255;
    range 192.168.0.128 192.168.0.254;
    ddns-domainname "internal.falcot.com";
}
```

### 10.8.2.Â DHCP and DNS

				A nice feature is the automated registering of DHCP clients in the DNS zone, so that each machine gets a significant name (rather than something impersonal such as machine-192-168-0-131.internal.falcot.com). Using this feature requires configuring the DNS server to accept updates to the internal.falcot.com DNS zone from the DHCP server, and configuring the latter to submit updates for each registration.
			
				In the bind case (see [SectionÂ 10.7.1, âDNS softwareâ](sect.domain-name-servers.en.md#sect.dns-software)), the allow-update directive needs to be added to each of the zones that the DHCP server is to edit (the one for the internal.falcot.com domain, and the reverse zone). This directive lists the IP addresses allowed to perform these updates; it should therefore contain the possible addresses of the DHCP server (both the local address and the public address, if appropriate).
			

```bash
allow-update { 127.0.0.1 192.168.0.1 212.94.201.10 !any };
```

				Beware! A zone that can be modified will be changed by bind, and the latter will overwrite its configuration files at regular intervals. Since this automated procedure produces files that are less human-readable than manually-written ones, the Falcot administrators handle the internal.falcot.com domain with a delegated DNS server; this means the falcot.com zone file stays firmly under their manual control.
			
				The DHCP server configuration excerpt above already includes the directives required for DNS zone updates: they are the ddns-update-style interim; and ddns-domain-name "internal.falcot.com"; lines.
			[Prev10.7.Â Domain Name Servers (DNS)](sect.domain-name-servers.en.md)[Up](#)[Home](index.en.md)[Next10.9.Â Network Diagnosis Tools](sect.network-diagnosis-tools.en.md)