---
title: "10.1. Gateway"
source: "https://www.debian.org/doc/manuals/debian-handbook/network-infrastructure.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/network-infrastructure.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "10.1. Gateway"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.power-management.en.md)The Debian Administrator's Handbook[Next](sect.x509-cert.en.md)

# ChapterÂ 10.Â Network Infrastructure

[10.1. Gateway](network-infrastructure.en.md#sect.gateway)[10.2. X.509 certificates](sect.x509-cert.en.md)[10.2.1. Creating gratis trusted certificates](sect.x509-cert.en.md#sect.trusted-certs)[10.2.2. Public Key Infrastructure: easy-rsa](sect.x509-cert.en.md#sect.easy-rsa)[10.3. Virtual Private Network](sect.virtual-private-network.en.md)[10.3.1. OpenVPN](sect.virtual-private-network.en.md#sect.openvpn)[10.3.2. Virtual Private Network with SSH](sect.virtual-private-network.en.md#sect.ssh-vpn)[10.3.3. IPsec](sect.virtual-private-network.en.md#sect.ipsec)[10.3.4. PPTP](sect.virtual-private-network.en.md#sect.pptp)[10.4. Quality of Service](sect.quality-of-service.en.md)[10.4.1. Principle and Mechanism](sect.quality-of-service.en.md#sect.qos-principe)[10.4.2. Configuring and Implementing](sect.quality-of-service.en.md#sect.qos-config)[10.5. Dynamic Routing](sect.dynamic-routing.en.md)[10.6. IPv6](sect.ipv6.en.md)[10.6.1. Tunneling](sect.ipv6.en.md#sect.ipv6-tunneling)[10.7. Domain Name Servers (DNS)](sect.domain-name-servers.en.md)[10.7.1. DNS software](sect.domain-name-servers.en.md#sect.dns-software)[10.7.2. Configuring bind](sect.domain-name-servers.en.md#sect.dns-config)[10.8. DHCP](sect.dhcp.en.md)[10.8.1. Configuring](sect.dhcp.en.md#sect.dhcp-config)[10.8.2. DHCP and DNS](sect.dhcp.en.md#sect.dhcp-dns)[10.9. Network Diagnosis Tools](sect.network-diagnosis-tools.en.md)[10.9.1. Local Diagnosis: netstat](sect.network-diagnosis-tools.en.md#sect.netstat)[10.9.2. Remote Diagnosis: nmap](sect.network-diagnosis-tools.en.md#sect.nmap)[10.9.3. Sniffers: tcpdump and wireshark](sect.network-diagnosis-tools.en.md#sect.sniffers)
		Linux sports the whole Unix heritage for networking, and Debian provides a full set of tools to create and manage them. This chapter reviews these tools.
	

## 10.1.Â Gateway

			A gateway is a system linking several networks. This term often refers to a local network's âexit pointâ on the mandatory path to all external IP addresses. The gateway is connected to each of the networks it links together, and acts as a router to convey IP packets between its various interfaces.
		

BACK TO BASICS IP packet

			Most networks nowadays use the IP protocol (Internet Protocol). This protocol segments the transmitted data into limited-size packets. Each packet contains, in addition to its payload data, a number of details required for its proper routing.
		

BACK TO BASICS TCP/UDP

			Many programs do not handle the individual packets themselves, even though the data they transmit does travel over IP; they often use TCP (Transmission Control Protocol). TCP is a layer over IP allowing the establishment of connections dedicated to data streams between two points. The programs then only see an entry point into which data can be fed with the guarantee that the same data exits without loss (and in the same sequence) at the exit point at the other end of the connection. Although many kinds of errors can happen in the lower layers, they are compensated by TCP: lost packets are retransmitted, and packets arriving out of order (for example, if they used different paths) are re-ordered appropriately.
		
			Another protocol relying on IP is UDP (User Datagram Protocol). In contrast to TCP, it is packet-oriented. Its goals are different: the purpose of UDP is only to transmit one packet from an application to another. The protocol does not try to compensate for possible packet loss on the way, nor does it ensure that packets are received in the same order as were sent. The main advantage to this protocol is that the latency is greatly improved, since the loss of a single packet does not delay the receiving of all following packets until the lost one is retransmitted.
		
			TCP and UDP both involve ports, which are âextension numbersâ for establishing communication with a given application on a machine. This concept allows keeping several different communications in parallel with the same correspondent, since these communications can be distinguished by the port number.
		
			Some of these port numbers â standardized by the IANA (Internet Assigned Numbers Authority) â are âwell-knownâ for being associated with network services. For instance, TCP portÂ 25 is generally used by the email server. The list of services associated with port numbers can be found in the /etc/services file, also explained in services(5), as well as in: âÂ [https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml](https://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml)

			When a local network uses a private address range (not routable on the Internet), the gateway needs to implement address masquerading so that the machines on the network can communicate with the outside world. The masquerading operation is a kind of proxy operating on the network level: each outgoing connection from an internal machine is replaced with a connection from the gateway itself (since the gateway does have an external, routable address), the data going through the masqueraded connection is sent to the new one, and the data coming back in reply is sent through to the masqueraded connection to the internal machine. The gateway uses a range of dedicated TCP ports for this purpose, usually with very high numbers (over 60000). Each connection coming from an internal machine then appears to the outside world as a connection coming from one of these reserved ports.
		

CULTURE Private address range

			RFCÂ 1918 defines three ranges of IPv4 addresses not meant to be routed on the Internet but only used in local networks. The first one, 10.0.0.0/8 (see sidebar [BACK TO BASICS Essential network concepts (Ethernet, IP address, subnet, broadcast)](sect.network-config.en.md#sidebar.networking-basics)), is a class-A range (with 224 IP addresses). The second one, 172.16.0.0/12, gathers 16 class-B ranges (172.16.0.0/16 to 172.31.0.0/16), each containing 216 IP addresses. Finally, 192.168.0.0/16 is a class-B range (grouping 256 class-C ranges, 192.168.0.0/24 to 192.168.255.0/24, with 256 IP addresses each). âÂ [http://www.faqs.org/rfcs/rfc1918.html](http://www.faqs.org/rfcs/rfc1918.html)

			The gateway can also perform two kinds of network address translation (or NAT for short). The first kind, Destination NAT (DNAT) is a technique to alter the destination IP address (and/or the TCP or UDP port) for a (generally) incoming connection. The connection tracking mechanism also alters the following packets in the same connection to ensure continuity in the communication. The second kind of NAT is Source NAT (SNAT), of which masquerading is a particular case; SNAT alters the source IP address (and/or the TCP or UDP port) of a (generally) outgoing connection. As for DNAT, all the packets in the connection are appropriately handled by the connection tracking mechanism. Note that NAT is only relevant for IPv4 and its limited address space; in IPv6, the wide availability of addresses greatly reduces the usefulness of NAT by allowing all âinternalâ addresses to be directly routable on the Internet (this does not imply that internal machines are accessible, since intermediary firewalls can filter traffic).
		

BACK TO BASICS Port forwarding

			A concrete application of DNAT is port forwarding. Incoming connections to a given port of a machine are forwarded to a port on another machine. Other solutions may exist for achieving a similar effect, though, especially at the application level with ssh (see [SectionÂ 9.2.1.4, âCreating Encrypted Tunnels with Port Forwardingâ](sect.remote-login.en.md#sect.ssh-port-forwarding)) or redir.
		
			Enough theory, let's get practical. Turning a Debian system into a gateway is a simple matter of enabling the appropriate option in the Linux kernel, by way of the /proc/ virtual filesystem:
		

```bash
# echo 1 > /proc/sys/net/ipv4/conf/default/forwarding
```

			This option can also be automatically enabled on boot if /etc/sysctl.conf or a configuration file in /etc/sysctl.d/ sets the net.ipv4.conf.default.forwarding option to 1.
		

ExampleÂ 10.1.Â The /etc/sysctl.conf file

```bash
net.ipv4.conf.default.forwarding = 1
net.ipv4.conf.default.rp_filter = 1
net.ipv4.tcp_syncookies = 1
```

			The same effect can be obtained for IPv6 by simply replacing ipv4 with ipv6 in the manual command and using the net.ipv6.conf.all.forwarding line in /etc/sysctl.conf.
		
			Enabling IPv4 masquerading is a slightly more complex operation that involves configuring the netfilter firewall.
		
			Similarly, using NAT (for IPv4) requires configuring netfilter. Since the primary purpose of this component is packet filtering, the details are listed in [ChapterÂ 14: âSecurityâ](security.en.md) (see [SectionÂ 14.2, âFirewall or Packet Filteringâ](sect.firewall-packet-filtering.en.md)).
		[Prev9.12.Â Power Management: Advanced Configuration an...](sect.power-management.en.md)[Up](#)[Home](index.en.md)[Next10.2.Â X.509 certificates](sect.x509-cert.en.md)