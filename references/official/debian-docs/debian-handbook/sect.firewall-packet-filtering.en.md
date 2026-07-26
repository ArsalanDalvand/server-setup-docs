---
title: "14.2.4. Installing the Rules at Each Boot"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.firewall-packet-filtering.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.firewall-packet-filtering.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "14.2.4. Installing the Rules at Each Boot"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](security.en.md)The Debian Administrator's Handbook[Next](sect.supervision.en.md)

## 14.2.Â Firewall or Packet Filtering

BACK TO BASICS Firewall

			A firewall is a piece of computer equipment with hardware and/or software that sorts the incoming or outgoing network packets (coming to or from a local network) and only lets through those matching certain predefined conditions.
		
			A firewall is a filtering network gateway and is only effective on packets that must go through it. Therefore, it can only be effective when going through the firewall is the only route for these packets.
		

SPECIFIC CASE Local Firewall

			A firewall can be restricted to one particular machine (as opposed to a complete network), in which case its role is to filter or limit access to some services. It is, however, always better to configure services to not listen on those network interfaces, where these services are not supposed to be offered, or to disable and remove services, which are not to be offered at all, than to use a packet filter to prevent any unwanted access to these services.
		
			Its role can also be to filter or limit connections by rogue or poorly programmed software that a user could, willingly or not, have installed. The reliability of this, however, highly depends on the goal of these actions and the integrity of the system itself. This action is not a measurement to prevent potential threats to send data.
		
			The Linux kernel embeds the netfilter firewall, which can be controlled from user space with the iptables, ip6tables, arptables and ebtables commands.
		
			However, Netfilter iptables commands are being replaced by nftables, which avoids many of its problems. Its design involves less code duplication, and it can be managed with just the nft command. Since Debian Buster, the nftables framework is used by default. The commands mentioned before are provided by versions, which use the nftables kernel API, by default. If one requires the âclassicâ commands, the relevant binaries can be adjusted using update-alternatives.
		

TOOLS fwbuilder and ufw

			Although (destined to) being replaced by nftables, iptables is still widely used and suited for many use-cases. Creating a rule requires an invocation of iptables/ip6tables. Typing these commands manually can be tedious, so the calls are usually stored in a script, so that the same configuration is set up automatically every time the machine boots, or they can be made âpersistentâ using iptables-persistent.
		
			A script usually needs to be written by hand. But there are, tools that make it simpler to configure the netfilter firewall, with a graphical representation of the filtering rules. fwbuilder is undoubtedly among the best of them. The rules are created with simple drag-and-drop actions on objects (interfaces, networks, ports, servers, etc.). A few contextual menus can change the condition (negating it, for instance). Then the action needs to be chosen and configured.
		
			An alternative to writing and saving/loading the required rules is to use ufw from the package with the same name. This tool is a frontend to iptables as well, but it provides a command line interface only. Simple commands can enable or disable (block) network traffic to and from ports and IP addresses. The configuration files in /etc/ufw/ also allow for complex rule sets (e.g. hooking into the Docker chains), which are then saved and loaded automatically. This tool is often used for simple setups and to block
		
			To enable a default firewall in Debian execute:
		

```bash
# apt install -y nftables
Reading package lists... Done
...
# systemctl enable nftables.service
Created symlink /etc/systemd/system/sysinit.target.wants/nftables.service â /lib/systemd/system/nftables.service.
```

### 14.2.1.Â nftables Behavior

				As the kernel is processing a network packet, it pauses and allows us to inspect the packet and decide what to do with that packet. For example, we might want to drop or discard certain incoming packets, modify other packets in various ways, block certain outgoing packets to control against malware or redirect some packets at the earliest possible stage to bridge network interfaces or to spread the load of incoming packets between systems.
			
				A good understanding of the layers 3, 4 and 5 of the OSI (Open Systems Interconnection) model is essential to get the most from netfilter.
			

CULTURE The OSI model

				The OSI model is a conceptual model to implement networking protocols without regard to its underlying internal structure and technology. Its goal is the interoperability of diverse communication systems with standard communication protocols.
			
				This model was defined in the standard ISO/EIC 7498. The following seven layers are described:
			
						Physical: transmission and reception of raw bit streams over a physical medium
					
						Data Link: reliable transmission of data frames between two nodes connected by a physical layer
					
						Network: structuring and managing a multi-node network, including addressing, routing and traffic control
					
						Transport: reliable transmission of data segments between points on a network, including segmentation, acknowledgment and multiplexing
					
						Session: managing communication sessions, i.e. continuous exchange of information in the form of multiple back-and-forth transmissions between two nodes
					
						Presentation: translation of data between a networking service and an application; including character encoding, data compression and encryption/decryption
					
						Application: High-level APIs, including resource sharing, remote file access.
					
				More information can be found on Wikipedia: âÂ [https://en.wikipedia.org/wiki/OSI_model](https://en.wikipedia.org/wiki/OSI_model)

				The firewall is configured with tables, which hold rules contained in chains. Unlike iptables, nftables does not have any default table. The user decides which and how many tables to create. Every table must have only one of the following five families assigned: ip, ip6, inet, arp and bridge. ip is used if the family is not specified.
			
				There are two types of chains: base chains and regular chains. A base chain is an entry point for packets from the networking stack. Base chains are registered into the Netfilter hooks, e.g. they see packets flowing through the TCP/IP stack. On the other hand, a regular chain is not attached to any hook so it does not see any traffic. But it may be used as a jump target for better organization of the rules.
			
				Rules are made of statements, which includes some expressions to be matched and then a verdict statement, like accept, drop, queue, continue, return, jump chain and goto chain.
			

BACK TO BASICS ICMP

				ICMP (Internet Control Message Protocol) is the protocol used to transmit complementary information on communications. It allows testing network connectivity with the ping command (which sends an ICMP echo request message, which the recipient is meant to answer with an ICMP echo reply message). It signals a firewall rejecting a packet, indicates an overflow in a receive buffer, proposes a better route for the next packets in the connection, and so on. This protocol is defined by several RFC documents; the initial RFC 777 and RFC 792 were soon completed and extended. âÂ [http://www.faqs.org/rfcs/rfc777.html](http://www.faqs.org/rfcs/rfc777.html) âÂ [http://www.faqs.org/rfcs/rfc792.html](http://www.faqs.org/rfcs/rfc792.html)

				For reference, a receive buffer is a small memory zone storing data between the time it arrives from the network and the time the kernel handles it. If this zone is full, new data cannot be received, and ICMP signals the problem, so that the emitter can slow down its transfer rate (which should ideally reach an equilibrium after some time).
			
				Note that although an IPv4 network can work without ICMP, ICMPv6 is strictly required for an IPv6 network, since it combines several functions that were, in the IPv4 world, spread across ICMPv4, IGMP (Internet Group Membership Protocol) and ARP (Address Resolution Protocol). ICMPv6 is defined in RFC 4443. âÂ [http://www.faqs.org/rfcs/rfc4443.html](http://www.faqs.org/rfcs/rfc4443.html)

### 14.2.2.Â Moving from iptables to nftables

				The iptables-translate and ip6tables-translate commands can be used to translate old iptables commands into the new nftables syntax. Whole rule sets can also be translated, in this case we migrate the rules configured in one computer which has Docker installed:
			

```bash
# iptables-save > iptables-ruleset.txt
# iptables-restore-translate -f iptables-ruleset.txt

# Translated by iptables-restore-translate v1.8.7 on Wed Mar 16 22:06:32 2022
add table ip filter
add chain ip filter INPUT { type filter hook input priority 0; policy accept; }
add chain ip filter FORWARD { type filter hook forward priority 0; policy drop; }
add chain ip filter OUTPUT { type filter hook output priority 0; policy accept; }
add chain ip filter DOCKER
add chain ip filter DOCKER-ISOLATION-STAGE-1
add chain ip filter DOCKER-ISOLATION-STAGE-2
add chain ip filter DOCKER-USER
add rule ip filter FORWARD counter jump DOCKER-USER
add rule ip filter FORWARD counter jump DOCKER-ISOLATION-STAGE-1
add rule ip filter FORWARD oifname "docker0" ct state related,established counter accept
add rule ip filter FORWARD oifname "docker0" counter jump DOCKER
add rule ip filter FORWARD iifname "docker0" oifname != "docker0" counter accept
add rule ip filter FORWARD iifname "docker0" oifname "docker0" counter accept
add rule ip filter DOCKER-ISOLATION-STAGE-1 iifname "docker0" oifname != "docker0" counter jump DOCKER-ISOLATION-STAGE-2
add rule ip filter DOCKER-ISOLATION-STAGE-1 counter return
add rule ip filter DOCKER-ISOLATION-STAGE-2 oifname "docker0" counter drop
add rule ip filter DOCKER-ISOLATION-STAGE-2 counter return
add rule ip filter DOCKER-USER counter return
add table ip nat
add chain ip nat PREROUTING { type nat hook prerouting priority -100; policy accept; }
add chain ip nat INPUT { type nat hook input priority 100; policy accept; }
add chain ip nat OUTPUT { type nat hook output priority -100; policy accept; }
add chain ip nat POSTROUTING { type nat hook postrouting priority 100; policy accept; }
add chain ip nat DOCKER
add rule ip nat PREROUTING fib daddr type local counter jump DOCKER
add rule ip nat OUTPUT ip daddr != 127.0.0.0/8 fib daddr type local counter jump DOCKER
add rule ip nat POSTROUTING oifname != "docker0" ip saddr 172.17.0.0/16 counter masquerade
add rule ip nat DOCKER iifname "docker0" counter return
# Completed on Wed Mar 16 22:06:32 2022
# iptables-restore-translate -f iptables-ruleset.txt > ruleset.nft
# nft -f ruleset.nft
# nft list ruleset
table inet filter {
	chain input {
		type filter hook input priority filter; policy accept;
	}

	chain forward {
		type filter hook forward priority filter; policy accept;
	}

	chain output {
		type filter hook output priority filter; policy accept;
	}
}
table ip nat {
	chain DOCKER {
		iifname "docker0" counter packets 0 bytes 0 return
		iifname "docker0" counter packets 0 bytes 0 return
	}

	chain POSTROUTING {
		type nat hook postrouting priority srcnat; policy accept;
		oifname != "docker0" ip saddr 172.17.0.0/16 counter packets 0 bytes 0 masquerade
		oifname != "docker0" ip saddr 172.17.0.0/16 counter packets 0 bytes 0 masquerade
	}

	chain PREROUTING {
		type nat hook prerouting priority dstnat; policy accept;
		fib daddr type local counter packets 1 bytes 60 jump DOCKER
		fib daddr type local counter packets 0 bytes 0 jump DOCKER
	}

	chain OUTPUT {
		type nat hook output priority -100; policy accept;
		ip daddr != 127.0.0.0/8 fib daddr type local counter packets 0 bytes 0 jump DOCKER
		ip daddr != 127.0.0.0/8 fib daddr type local counter packets 0 bytes 0 jump DOCKER
	}

	chain INPUT {
		type nat hook input priority 100; policy accept;
	}
}
table ip filter {
	chain DOCKER {
	}

	chain DOCKER-ISOLATION-STAGE-1 {
		iifname "docker0" oifname != "docker0" counter packets 0 bytes 0 jump DOCKER-ISOLATION-STAGE-2
		counter packets 0 bytes 0 return
		iifname "docker0" oifname != "docker0" counter packets 0 bytes 0 jump DOCKER-ISOLATION-STAGE-2
		counter packets 0 bytes 0 return
	}

	chain DOCKER-ISOLATION-STAGE-2 {
		oifname "docker0" counter packets 0 bytes 0 drop
		counter packets 0 bytes 0 return
		oifname "docker0" counter packets 0 bytes 0 drop
		counter packets 0 bytes 0 return
	}

	chain FORWARD {
		type filter hook forward priority filter; policy drop;
		counter packets 0 bytes 0 jump DOCKER-USER
		counter packets 0 bytes 0 jump DOCKER-ISOLATION-STAGE-1
		oifname "docker0" ct state related,established counter packets 0 bytes 0 accept
		oifname "docker0" counter packets 0 bytes 0 jump DOCKER
		iifname "docker0" oifname != "docker0" counter packets 0 bytes 0 accept
		iifname "docker0" oifname "docker0" counter packets 0 bytes 0 accept
		counter packets 0 bytes 0 jump DOCKER-USER
		counter packets 0 bytes 0 jump DOCKER-ISOLATION-STAGE-1
		oifname "docker0" ct state established,related counter packets 0 bytes 0 accept
		oifname "docker0" counter packets 0 bytes 0 jump DOCKER
		iifname "docker0" oifname != "docker0" counter packets 0 bytes 0 accept
		iifname "docker0" oifname "docker0" counter packets 0 bytes 0 accept
	}

	chain DOCKER-USER {
		counter packets 0 bytes 0 return
		counter packets 0 bytes 0 return
	}

	chain INPUT {
		type filter hook input priority filter; policy accept;
	}

	chain OUTPUT {
		type filter hook output priority filter; policy accept;
	}
}
```

				The tools iptables-nft, ip6tables-nft, arptables-nft, ebtables-nft are versions of iptables that use the nftables API, so users can keep using the old iptables syntax with them, but that is not recommended; these tools should only be used for backwards compatibility.
			

### 14.2.3.Â Syntax of nft

				The nft commands allow manipulating tables, chains and rules. The table option supports multiple operations: add, create, delete, list and flush. nft add table ip6 mangle adds a new table from the family ip6.
			
				To insert a new base chain to the filter table, you can execute the following command (note that the semicolon is escaped with a backslash when using Bash):
			

```bash
# nft add chain filter input { type filter hook input priority 0 \; }
```

				Rules are usually added with the following syntax: nft add rule [family] table chain handle handle statement.
			
insert is similar to the add command, but the given rule is prepended to the beginning of the chain or before the rule with the given handle instead of at the end or after that rule. For example, the following command inserts a rule before the rule with handler number 8:
			

```bash
# nft insert rule filter output position 8 ip daddr 127.0.0.8 drop
```

				The executed nft commands do not make permanent changes to the configuration, so they are lost if they are not saved. The firewall rules are located in /etc/nftables.conf. A simple way to save the current firewall configuration permanently is to execute nft list ruleset > /etc/nftables.conf as root.
			
nft allows many more operations, refer to its manual page nft(8) for more information.
			

### 14.2.4.Â Installing the Rules at Each Boot

				To enable a default firewall in Debian, you need to store the rules in /etc/nftables.conf and execute systemctl enable nftables as root. You can stop the firewall by executing nft flush ruleset as root.
			
				In other cases, the recommended way is to register the configuration script in up directive of the /etc/network/interfaces file. In the following example, the script is stored under /usr/local/etc/arrakis.fw.
			

ExampleÂ 14.1.Â interfaces file calling firewall script

```bash
auto eth0
iface eth0 inet static
    address 192.168.0.1
    network 192.168.0.0
    netmask 255.255.255.0
    broadcast 192.168.0.255
    up /usr/local/etc/arrakis.fw
```

				This obviously assumes that you are using ifupdown to configure the network interfaces. If you are using something else (like NetworkManager or systemd-networkd), then refer to their respective documentation to find out ways to execute a script after the interface has been brought up.
			[PrevChapterÂ 14.Â Security](security.en.md)[Up](#)[Home](index.en.md)[Next14.3.Â Supervision: Prevention, Detection, Deterre...](sect.supervision.en.md)