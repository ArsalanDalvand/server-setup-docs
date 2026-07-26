---
title: "10.5. Dynamic Routing"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.dynamic-routing.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.dynamic-routing.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "10.5. Dynamic Routing"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.quality-of-service.en.md)The Debian Administrator's Handbook[Next](sect.ipv6.en.md)

## 10.5.Â Dynamic Routing

			The reference tool for dynamic routing is currently frr from the similarly-named package; it used to be quagga, and before that zebra until development of these stopped. However, frr kept the names of the programs for compatibility reasons which explains the zebra commands below.
		

BACK TO BASICS Dynamic routing

			Dynamic routing allows routers to adjust, in real time, the paths used for transmitting IP packets. Each protocol involves its own method of defining routes (shortest path, use routes advertised by peers, and so on).
		
			In the Linux kernel, a route links a network device to a set of machines that can be reached through this device. The ip command, when route is used as the first argument, defines new routes and displays existing ones. The route command was used for that purpose, but it is deprecated in favor of ip.
		
			FRR (FRRouting) is a set of daemons cooperating to define the routing tables to be used by the Linux kernel; each routing protocol (most notably BGP, OSPF and RIP) provides its own daemon(s). The zebra and staticd daemons, which are always started, collect information from other daemons and handle static routing tables accordingly. The other daemons are known as bgpd, ospfd, ospf6d, ripd, ripngd, isisd, etc.
		
			Daemons are enabled by creating the /etc/frr/daemon.conf config file, daemon being the name of the daemon to use, and editing the /etc/frr/daemon configuration file. The daemon config file must belong to the frr user and group with permissions of 0640 in order for the /etc/init.d/frr script or the frr.service systemd service file to invoke the daemon. The package frr provides configuration examples under /usr/share/doc/frr/examples/.
		
			The configuration of each of these daemons requires knowledge of the routing protocol in question. These protocols cannot be described in detail here, but frr-doc provides ample explanation in the form of both info and HTML files. The same contents may be more browsed on the project's website: âÂ [http://docs.frrouting.org/en/latest/](http://docs.frrouting.org/en/latest/)

			In addition, the syntax is very close to a standard router's configuration interface, and network administrators will adapt quickly to frr.
		

IN PRACTICE OSPF, BGP or RIP?

OSPF (Open Shortest Path First) is generally the best protocol to use for dynamic routing on private networks, but BGP (Border Gateway Protocol) is more common for Internet-wide routing. RIP (Routing Information Protocol) is rather ancient, and hardly used anymore.
		[Prev10.4.Â Quality of Service](sect.quality-of-service.en.md)[Up](#)[Home](index.en.md)[Next10.6.Â IPv6](sect.ipv6.en.md)