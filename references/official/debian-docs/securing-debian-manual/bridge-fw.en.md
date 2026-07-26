---
title: "B.4.3. Basic IPtables rules"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/bridge-fw.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/bridge-fw.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "B.4.3. Basic IPtables rules"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](snort-box.en.md)Securing Debian Manual[Next](bind-chuser.en.md)

## B.4.Â Setting up a bridge firewall

			This information was contributed by Francois Bayart in order to help users set up a Linux bridge/firewall with the 2.4.x kernel and iptables. Kernel patches are no more needed as the code was made standard part of the Linux kernel distribution. 
		
			To configure the kernel with necessary support, run make menuconfig or make xconfig. In the section Networking options, enable the following options: 

```bash
[*] Network packet filtering (replaces ipchains)
[ ]   Network packet filtering debugging (NEW)
<*> 802.1d Ethernet Bridging
[*]   netfilter (firewalling) support (NEW)
```

			Caution: you must disable this if you want to apply some firewalling rules or else iptables will not work: 

```bash
[ ]   Network packet filtering debugging (NEW)
```

			Next, add the correct options in the section IP: Netfilter Configuration. Then, compile and install the kernel. If you want to do it the Debian way, install kernel-package and run make-kpkg to create a custom Debian kernel package you can install on your server using dpkg. Once the new kernel is compiled and installed, install the bridge-utils package.
		
			Once these steps are complete, you can complete the configuration of your bridge. The next section presents two different possible configurations for the bridge, each with a hypothetical network map and the necessary commands.
		

### B.4.1.Â A bridge providing NAT and firewall capabilities

				The first configuration uses the bridge as a firewall with network address translation (NAT) that protects a server and internal LAN clients. A diagram of the network configuration is shown below: 

```bash
Internet ---- router ( 62.3.3.25 ) ---- bridge (62.3.3.26 gw 62.3.3.25 / 192.168.0.1)
                                          |
                                          |
                                          |---- WWW Server (62.3.3.27 gw 62.3.3.25)
                                          |
                                          |
                                         LAN --- Zipowz (192.168.0.2 gw 192.168.0.1)
```

				The following commands show how this bridge can be configured. 

```bash
# Create the interface br0
/usr/sbin/brctl addbr br0

# Add the Ethernet interface to use with the bridge
/usr/sbin/brctl addif br0 eth0
/usr/sbin/brctl addif br0 eth1

# Start up the Ethernet interface
/sbin/ifconfig eth0 0.0.0.0
/sbin/ifconfig eth1 0.0.0.0

# Configure the bridge ethernet
# The bridge will be correct and invisible ( transparent firewall ).
# It's hidden in a traceroute and you keep your real gateway on the 
# other computers. Now if you want you can config a gateway on your 
# bridge and choose it as your new gateway for the other computers.

/sbin/ifconfig br0 62.3.3.26 netmask 255.255.255.248 broadcast 62.3.3.31

# I have added this internal IP to create my NAT 
ip addr add 192.168.0.1/24 dev br0
/sbin/route add default gw 62.3.3.25
```

### B.4.2.Â A bridge providing firewall capabilities

				A second possible configuration is a system that is set up as a transparent firewall for a LAN with a public IP address space. 

```bash
Internet ---- router (62.3.3.25) ---- bridge (62.3.3.26)
                                        |
                                        |
                                        |---- WWW Server (62.3.3.28 gw 62.3.3.25)
                                        |
                                        |
                                        |---- Mail Server (62.3.3.27 gw 62.3.3.25)
```

				The following commands show how this bridge can be configured. 

```bash
# Create the interface br0
/usr/sbin/brctl addbr br0

# Add the Ethernet interface to use with the bridge
/usr/sbin/brctl addif br0 eth0
/usr/sbin/brctl addif br0 eth1

# Start up the Ethernet interface
/sbin/ifconfig eth0 0.0.0.0
/sbin/ifconfig eth1 0.0.0.0

# Configure the bridge Ethernet
# The bridge will be correct and invisible ( transparent firewall ).
# It's hidden in a traceroute and you keep your real gateway on the 
# other computers. Now if you want you can config a gateway on your
# bridge and choose it as your new gateway for the other computers.

/sbin/ifconfig br0 62.3.3.26 netmask 255.255.255.248 broadcast 62.3.3.31
```

				If you traceroute the Linux Mail Server, you won't see the bridge. If you want access to the bridge with ssh, you must have a gateway or you must first connect to another server, such as the "Mail Server", and then connect to the bridge through the internal network card.
			

### B.4.3.Â Basic IPtables rules

				This is an example of the basic rules that could be used for either of these setups. 
				

ExampleÂ B.1.Â Basic Iptables rules

```bash
iptables -F FORWARD
iptables -P FORWARD DROP
iptables -A FORWARD -s 0.0.0.0/0.0.0.0 -d 0.0.0.0/0.0.0.0 -m state --state INVALID -j DROP
iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT

# Some funny rules but not in a classic Iptables sorry ...
# Limit ICMP 
# iptables -A FORWARD -p icmp -m limit --limit 4/s -j ACCEPT
# Match string, a good simple method to block some VIRUS very quickly
# iptables -I FORWARD -j DROP -p tcp -s 0.0.0.0/0 -m string --string "cmd.exe"

# Block all MySQL connection just to be sure
iptables -A FORWARD -p tcp -s 0/0 -d 62.3.3.0/24 --dport 3306 -j DROP

# Linux Mail Server Rules

# Allow FTP-DATA (20), FTP (21), SSH (22) 
iptables -A FORWARD -p tcp -s 0.0.0.0/0 -d 62.3.3.27/32 --dport 20:22 -j ACCEPT

# Allow the Mail Server to connect to the outside
# Note: This is *not* needed for the previous connections 
# (remember: stateful filtering) and could be removed.
iptables -A FORWARD -p tcp -s 62.3.3.27/32 -d 0/0 -j ACCEPT

# WWW Server Rules

# Allow HTTP ( 80 ) connections with the WWW server
iptables -A FORWARD -p tcp -s 0.0.0.0/0 -d 62.3.3.28/32 --dport 80 -j ACCEPT

# Allow HTTPS ( 443 ) connections with the WWW server
iptables -A FORWARD -p tcp -s 0.0.0.0/0 -d 62.3.3.28/32 --dport 443 -j ACCEPT

# Allow the WWW server to go out
# Note: This is *not* needed for the previous connections 
# (remember: stateful filtering) and could be removed.
iptables -A FORWARD -p tcp -s 62.3.3.28/32 -d 0/0 -j ACCEPT
```

[PrevB.3.Â Setting up a stand-alone IDS](snort-box.en.md)[Up](#)[Home](index.en.md)[NextB.5.Â Sample script to change the default Bind ins...](bind-chuser.en.md)