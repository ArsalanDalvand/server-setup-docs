---
title: "4.18.6. Protecting against ARP attacks"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/network-secure.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/network-secure.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.18.6. Protecting against ARP attacks"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch04s17.en.md)Securing Debian Manual[Next](snapshot.en.md)

## 4.18.Â Securing network access

			FIXME: More (Debian-specific) content needed.
		

### 4.18.1.Â Configuring kernel network features

				Many features of the kernel can be modified while running by echoing something into the /proc file system or by using sysctl. By entering /sbin/sysctl -A you can see what you can configure and what the options are, and it can be modified running 

```bash
/sbin/sysctl -w variable=value
```

				 (see sysctl(8)). Only in rare cases do you need to edit something here, but you can increase security that way as well. For example:  

```bash
net/ipv4/icmp_echo_ignore_broadcasts = 1
```

				This is a Windows emulator because it acts like Windows on broadcast ping if this option is set to 1. That is, ICMP echo requests sent to the broadcast address will be ignored. Otherwise, it does nothing.
			
				If you want to prevent you system from answering ICMP echo requests, just enable this configuration option: 

```bash
net/ipv4/icmp_echo_ignore_all = 1
```

				To log packets with impossible addresses (due to wrong routes) on your network use: 

```bash
/proc/sys/net/ipv4/conf/all/log_martians = 1
```

				For more information on what things can be done with /proc/sys/net/ipv4/* read /usr/src/linux/Documentation/filesystems/proc.txt. All the options are described thoroughly under /usr/src/linux/Documentation/networking/ip-sysctl.txt[[31]](#ftn.idm2122).
			

### 4.18.2.Â Configuring syncookies

				This option is a double-edged sword. On the one hand it protects your system against syn packet flooding; on the other hand it violates defined standards (RFCs).  

```bash
net/ipv4/tcp_syncookies = 1
```

				If you want to change this option each time the kernel is working you need to change it in /etc/network/options by setting syncookies=yes. This will take effect when ever /etc/init.d/networking is run (which is typically done at boot time) while the following will have a one-time effect until the reboot: 

```bash
echo 1 > /proc/sys/net/ipv4/tcp_syncookies
```

				This option will only be available if the kernel is compiled with the CONFIG_SYNCOOKIES. All Debian kernels are compiled with this option builtin but you can verify it running: 

```bash
$ sysctl -A |grep syncookies
net/ipv4/tcp_syncookies = 1
```

				For more information on TCP syncookies read [http://cr.yp.to/syncookies.html](http://cr.yp.to/syncookies.html).
			

### 4.18.3.Â Securing the network on boot-time

				When setting configuration options for the kernel networking you need configure it so that it's loaded every time the system is restarted. The following example enables many of the previous options as well as other useful options.
			
				There are actually two ways to configure your network at boot time. You can configure /etc/sysctl.conf (see: sysctl.conf(5)) or introduce a script that is called when the interface is enabled. The first option will be applied to all interfaces, whileas the second option allows you to configure this on a per-interface basis.
			
				An example of a /etc/sysctl.conf configuration that will secure some network options at the kernel level is shown below. Notice the comment in it, /etc/network/options might override some values if they contradict those in this file when the /etc/init.d/networking is run (which is later than procps on the startup sequence). 

```bash
#
# /etc/sysctl.conf - Configuration file for setting system variables
# See sysctl.conf (5) for information. Also see the files under
# Documentation/sysctl/, Documentation/filesystems/proc.txt, and
# Documentation/networking/ip-sysctl.txt in the kernel sources 
# (/usr/src/kernel-$version if you have a kernel-package installed)
# for more information of the values that can be defined here.

#
# Be warned that /etc/init.d/procps is executed to set the following
# variables.  However, after that, /etc/init.d/networking sets some
# network options with builtin values.  These values may be overridden
# using /etc/network/options.
#
#kernel.domainname = example.com

# Additional settings - adapted from the script contributed
# by Dariusz Puchala (see below)
# Ignore ICMP broadcasts
net/ipv4/icmp_echo_ignore_broadcasts = 1
#
# Ignore bogus ICMP errors
net/ipv4/icmp_ignore_bogus_error_responses = 1
# 
# Do not accept ICMP redirects (prevent MITM attacks)
net/ipv4/conf/all/accept_redirects = 0
# _or_
# Accept ICMP redirects only for gateways listed in our default
# gateway list (enabled by default)
# net/ipv4/conf/all/secure_redirects = 1
#
# Do not send ICMP redirects (we are not a router)
net/ipv4/conf/all/send_redirects = 0
#
# Do not forward IP packets (we are not a router)
# Note: Make sure that /etc/network/options has 'ip_forward=no'
net/ipv4/conf/all/forwarding = 0
#
# Enable TCP Syn Cookies
# Note: Make sure that /etc/network/options has 'syncookies=yes'
net/ipv4/tcp_syncookies = 1
#
# Log Martian Packets
net/ipv4/conf/all/log_martians = 1
#
# Turn on Source Address Verification in all interfaces to
# prevent some spoofing attacks
# Note: Make sure that /etc/network/options has 'spoofprotect=yes'
net/ipv4/conf/all/rp_filter = 1
#
# Do not accept IP source route packets (we are not a router)
net/ipv4/conf/all/accept_source_route = 0
```

				To use the script you need to first create the script, for example, in /etc/network/interface-secure (the name is given as an example) and call it from /etc/network/interfaces like this: 

```bash
auto eth0
iface eth0 inet static
        address xxx.xxx.xxx.xxx
        netmask 255.255.255.xxx
        broadcast xxx.xxx.xxx.xxx
        gateway xxx.xxx.xxx.xxx
        pre-up /etc/network/interface-secure
```

				In this example, before the interface eth0 is enabled the script will be called to secure all network interfaces as shown below. 

```bash
#!/bin/sh -e
# Script-name: /etc/network/interface-secure
#
# Modifies some default behavior in order to secure against 
# some TCP/IP spoofing & attacks for all interfaces.
#
# Contributed by Dariusz Puchalak.
#
echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_broadcasts 
                                           # Broadcast echo protection enabled.
echo 0 > /proc/sys/net/ipv4/conf/all/forwarding
                                           # IP forwarding disabled.
echo 1 > /proc/sys/net/ipv4/tcp_syncookies # TCP syn cookies protection enabled.
echo 1 >/proc/sys/net/ipv4/conf/all/log_martians # Log strange packets.
# (this includes spoofed packets, source routed packets, redirect packets)
# but be careful with this on heavy loaded web servers.
echo 1 > /proc/sys/net/ipv4/icmp_ignore_bogus_error_responses 
                                           # Bad error message protection enabled.

# IP spoofing protection.
echo 1 > /proc/sys/net/ipv4/conf/all/rp_filter

# Disable ICMP redirect acceptance.
echo 0 > /proc/sys/net/ipv4/conf/all/accept_redirects
echo 0 > /proc/sys/net/ipv4/conf/all/send_redirects

# Disable source routed packets.
echo 0 > /proc/sys/net/ipv4/conf/all/accept_source_route

exit 0
```

				Notice that you can actually have per-interface scripts that will enable different network options for different interfaces (if you have more than one), just change the pre-up line to: 

```bash
pre-up /etc/network/interface-secure $IFACE
```

				And use a script which will only apply changes to a specific interface, not to all of the interfaces available. Notice that some networking options can only be enabled globally, however. A sample script is this one: 

```bash
#!/bin/sh -e
# Script-name: /etc/network/interface-secure
#
# Modifies some default behavior in order to secure against 
# some TCP/IP spoofing & attacks for a given interface.
#
# Contributed by Dariusz Puchalak.
#

IFACE=$1
if [ -z "$IFACE" ] ; then
   echo "$0: Must give an interface name as argument!"
   echo "Usage: $0 <interface>"
   exit 1
fi

if [ ! -e /proc/sys/net/ipv4/conf/$IFACE/ ]; then
   echo "$0: Interface $IFACE does not exit (cannot find /proc/sys/net/ipv4/conf/)"
   exit 1
fi

echo 0 > /proc/sys/net/ipv4/conf/$IFACE/forwarding  # IP forwarding disabled.
echo 1 >/proc/sys/net/ipv4/conf/$IFACE/log_martians # Log strange packets.
# (this includes spoofed packets, source routed packets, redirect packets)
# but be careful with this on heavy loaded web servers.

# IP spoofing protection.
echo 1 > /proc/sys/net/ipv4/conf/$IFACE/rp_filter

# Disable ICMP redirect acceptance.
echo 0 > /proc/sys/net/ipv4/conf/$IFACE/accept_redirects
echo 0 > /proc/sys/net/ipv4/conf/$IFACE/send_redirects

# Disable source routed packets.
echo 0 > /proc/sys/net/ipv4/conf/$IFACE/accept_source_route

exit 0
```

				An alternative solution is to create an init.d script and have it run on bootup (using update-rc.d to create the appropriate rc.d links).
			

### 4.18.4.Â Configuring firewall features

				In order to have firewall capabilities, either to protect the local system or others behind it, the kernel needs to be compiled with firewall capabilities. The standard Debian 2.2 kernel (Linux 2.2) provides the packet filter ipchains firewall, Debian 3.0 standard kernel (Linux 2.4) provides the stateful packet filter iptables (netfilter) firewall.
			
				In any case, it is pretty easy to use a kernel different from the one provided by Debian. You can find pre-compiled kernels as packages you can easily install in the Debian system. You can also download the kernel sources using the kernel-source-X and build custom kernel packages using make-kpkg from the kernel-package package.
			
				Setting up firewalls in Debian is discussed more thoroughly in [SectionÂ 5.14, âAdding firewall capabilitiesâ](firewall-setup.en.md).
			

### 4.18.5.Â Disabling weak-end hosts issues

				Systems with more than one interface on different networks can have services configured so that they will bind only to a given IP address. This usually prevents access to services when requested through any other address. However, this does not mean (although it is a common misconception) that the service is bound to a given hardware address (interface card). [[32]](#ftn.idm2189)

				It seems, however, not to work with services bound to 127.0.0.1, you might need to write the tests using raw sockets.
			
				This is not an ARP issue and it's not an RFC violation (it's called weak end host in [RFC1122](ftp://ftp.isi.edu/in-notes/rfc1122.txt), (in the section 3.3.4.2). Remember, IP addresses have nothing to do with physical interfaces.
			
				On 2.2 (and previous) kernels this can be fixed with: 

```bash
# echo 1 > /proc/sys/net/ipv4/conf/all/hidden
# echo 1 > /proc/sys/net/ipv4/conf/eth0/hidden
# echo 1 > /proc/sys/net/ipv4/conf/eth1/hidden
.....
```

				On later kernels this can be fixed either with: 
				
							iptables rules.
						
							properly configured routing. [[33]](#ftn.idm2204)

							kernel patching. [[34]](#ftn.idm2209)

				Along this text there will be many occasions in which it is shown how to configure some services (sshd server, apache, printer service...) in order to have them listening on any given address, the reader should take into account that, without the fixes given here, the fix would not prevent accesses from within the same (local) network. [[35]](#ftn.idm2214)

				FIXME: Comments on Bugtraq indicate there is a Linux specific method to bind to a given interface.
			
				FIXME: Submit a bug against netbase so that the routing fix is standard behavior in Debian?
			

### 4.18.6.Â Protecting against ARP attacks

				When you don't trust the other boxes on your LAN (which should always be the case, because it's the safest attitude) you should protect yourself from the various existing ARP attacks.
			
				As you know the ARP protocol is used to link IP addresses to MAC addresses (see [ftp://ftp.isi.edu/in-notes/rfc826.txt](ftp://ftp.isi.edu/in-notes/rfc826.txt) for all the details). Every time you send a packet to an IP address an ARP resolution is done (first by looking into the local ARP cache then if the IP isn't present in the cache by broadcasting an ARP query) to find the target's hardware address. All the ARP attacks aim to fool your box into thinking that box B's IP address is associated to the intruder's box's MAC address; Then every packet that you want to send to the IP associated to box B will be send to the intruder's box...
			
				Those attacks (ARP cache poisoning, ARP spoofing...) allow the attacker to sniff the traffic even on switched networks, to easily hijack connections, to disconnect any host from the network... ARP attacks are powerful and simple to implement, and several tools exists, such as arpspoof from the dsniff package or [http://arpoison.sourceforge.net/](http://arpoison.sourceforge.net/).
			
				However, there is always a solution: 
				
							Use a static ARP cache. You can set up "static" entries in your ARP cache with: 

```bash
arp -s host_name hdwr_addr
```

							 By setting static entries for each important host in your network you ensure that nobody will create/modify a (fake) entry for these hosts (static entries don't expire and can't be modified) and spoofed ARP replies will be ignored.
						
							Detect suspicious ARP traffic. You can use arpwatch, karpski or more general IDS that can also detect suspicious ARP traffic (snort, [http://www.prelude-ids.org](http://www.prelude-ids.org)...).
						
							Implement IP traffic filtering validating the MAC address.
						
[[31]](#idm2122)
					In Debian the kernel-source-version packages copy the sources to /usr/src/kernel-source-version.tar.bz2, just substitute version to whatever kernel version sources you have installed
				[[32]](#idm2189)
					To reproduce this (example provided by Felix von Leitner on the Bugtraq mailing list): 

```bash
host a (eth0 connected to eth0 of host b):
     ifconfig eth0 10.0.0.1
     ifconfig eth1 23.0.0.1
     tcpserver -RHl localhost 23.0.0.1 8000 echo fnord

   host b:
     ifconfig eth0 10.0.0.2
     route add 23.0.0.1 gw 10.0.0.1
     telnet 23.0.0.1 8000
```

[[33]](#idm2204)
								The fact that this behavior can be changed through routing was described by Matthew G. Marsh in the Bugtraq thread: 

```bash
eth0 = 1.1.1.1/24
eth1 = 2.2.2.2/24

ip rule add from 1.1.1.1/32 dev lo table 1 prio 15000
ip rule add from 2.2.2.2/32 dev lo table 2 prio 16000

ip route add default dev eth0 table 1
ip route add default dev eth1 table 2
```

[[34]](#idm2209)
								There are some patches available for this behavior as described in Bugtraq's thread at [http://www.linuxvirtualserver.org/~julian/#hidden](http://www.linuxvirtualserver.org/~julian/#hidden) and [http://www.fefe.de/linux-eth-forwarding.diff](http://www.fefe.de/linux-eth-forwarding.diff).
							[[35]](#idm2214)
					An attacker might have many problems pulling the access through after configuring the IP-address binding while not being on the same broadcast domain (same network) as the attacked host. If the attack goes through a router it might be quite difficult for the answers to return somewhere.
				[Prev4.17.Â File system limits and control](ch04s17.en.md)[Up](#)[Home](index.en.md)[Next4.19.Â Taking a snapshot of the system](snapshot.en.md)