---
title: "5.14.3. Setting up a firewall"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/firewall-setup.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/firewall-setup.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "5.14.3. Setting up a firewall"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](rpc.en.md)Securing Debian Manual[Next](automatic-harden.en.md)

## 5.14.Â Adding firewall capabilities

			The Debian GNU/Linux operating system has the built-in capabilities provided by the Linux kernel. If you install a recent Debian release (default kernel installed is 2.6) you will have iptables (netfilter) firewalling available[[43]](#ftn.idm2903).
		

### 5.14.1.Â Firewalling the local system

				You can use firewall rules as a way to secure the access to your local system and, even, to limit the outbound communications made by it. Firewall rules can also be used to protect processes that cannot be properly configured not to provide services to some networks, IP addresses, etc.
			
				However, this step is presented last in this manual basically because it is much better not to depend solely on firewalling capabilities in order to protect a given system. Security in a system is made up of layers, firewalling should be the last to include, once all services have been hardened. You can easily imagine a setup in which the system is solely protected by a built-in firewall and an administrator blissfully removes the firewall rules for whatever reason (problems with the setup, annoyance, human error...), this system would be wide open to an attack if there were no other hardening in the system to protect from it.
			
				On the other hand, having firewall rules on the local system also prevents some bad things from happening. Even if the services provided are configured securely, a firewall can protect from misconfigurations or from fresh installed services that have not yet been properly configured. Also, a tight configuration will prevent trojans calling home from working unless the firewalling code is removed. Note that an intruder does not need superuser access to install a trojan locally that could be remotely controlled (since binding on ports is allowed if they are not priviledged ports and capabilities have not been removed).
			
				Thus, a proper firewall setup would be one with a default deny policy, that is: 
				
							incoming connections are allowed only to local services by allowed machines.
						
							outgoing connections are only allowed to services used by your system (DNS, web browsing, POP, email...).[[44]](#ftn.idm2924)

							the forward rule denies everything (unless you are protecting other systems, see below).
						
							all other incoming or outgoing connections are denied.
						

### 5.14.2.Â Using a firewall to protect other systems

				A Debian firewall can also be installed in order to protect, with filtering rules, access to systems behind it, limiting their exposure to the Internet. A firewall can be configured to prevent access from systems outside of the local network to internal services (ports) that are not public. For example, on a mail server, only port 25 (where the mail service is being given) needs to be accessible from the outside. A firewall can be configured to, even if there are other network services besides the public ones running in the mail server, throw away packets (this is known as filtering) directed towards them.
			
				You can even set up a Debian GNU/Linux box as a bridge firewall, i.e. a filtering firewall completely transparent to the network that lacks an IP address and thus cannot be attacked directly. Depending on the kernel you have installed, you might need to install the bridge firewall patch and then go to 802.1d Ethernet Bridging when configuring the kernel and a new option netfilter ( firewalling ) support. See the [SectionÂ B.4, âSetting up a bridge firewall â](bridge-fw.en.md) for more information on how to set this up in a Debian GNU/Linux system.
			

### 5.14.3.Â Setting up a firewall

				The default Debian installation, unlike other Linux distributions, does not yet provide a way for the administrator to setup a firewall configuration throughout the default installation but you can install a number of firewall configuration packages (see [SectionÂ 5.14.3.1, âUsing firewall packagesâ](firewall-setup.en.md#firewall-pack)).
			
				Of course, the configuration of the firewall is always system and network dependant. An administrator must know beforehand what is the network layout and the systems to protect, the services that need to be accessed, and whether or not other network considerations (like NAT or routing) need to be taken into account. Be careful when configuring your firewall, as Laurence J. Lane says in the iptables package:
			
The tools can easily be misused, causing enormous amounts of grief by completely crippling network access to a system. It is not terribly uncommon for a remote system administrator to accidentally get locked out of a system hundreds or thousands of miles away. You can even manage to get locked out of a computer who's keyboard is under your own fingers. Please, use due caution.

				Remember this: just installing the iptables (or the older firewalling code) does not give you any protection, just provides the software. In order to have a firewall you need to configure it!
			
				If you do not have a clue on how to set up your firewall rules manually consult the Packet Filtering HOWTO and NAT HOWTO provided by iptables for offline reading at /usr/share/doc/iptables/html/.
			
				If you do not know much about firewalling you should start by reading the [http://www.tldp.org/HOWTO/Firewall-HOWTO.html](http://www.tldp.org/HOWTO/Firewall-HOWTO.html), install the doc-linux-text package if you want to read it offline. If you want to ask questions or need help setting up a firewall you can use the debian-firewall mailing list, see [http://lists.debian.org/debian-firewall](http://lists.debian.org/debian-firewall). Also see [SectionÂ 1.4, âPrior knowledgeâ](references.en.md) for more (general) pointers on firewalls. Another good iptables tutorial is [http://iptables-tutorial.frozentux.net/iptables-tutorial.html](http://iptables-tutorial.frozentux.net/iptables-tutorial.html).
			

#### 5.14.3.1.Â Using firewall packages

					Setting up manually a firewall can be complicated for novice (and sometimes even expert) administrators. However, the free software community has created a number of tools that can be used to easily configure a local firewall. Be forewarned that some of these tools are oriented more towards local-only protection (also known as personal firewall) and some are more versatile and can be used to configure complex rules to protect whole networks.
				
					Some software that can be used to set up firewall rules in a Debian system is: 
					
								For desktop systems: 
								
firestarter, a GNOME application oriented towards end-users that includes a wizard useful to quickly setup firewall rules. The application includes a GUI to be able to monitor when a firewall rule blocks traffic.
										
guarddog, a KDE based firewall configuration package oriented both to novice and advanced users.
										
knetfilter, a KDE GUI to manage firewall and NAT rules for iptables (alternative/competitor to the guarddog tool although slightly oriented towards advanced users).
										
											fireflier, an interactive tool to create iptables rules based on traffic seen on the system and applications. It has a server-client model so you have to install both the server (fireflier-server) and one of the available clients, with one client available for different desktop environments: fireflier-client-gtk (Gtk+ client), fireflier-client-kde (KDE client) and fireflier-client-qt (QT client).
										

								For servers (headless) systems: 
								
fwbuilder, an object oriented GUI which includes policy compilers for various firewall platforms including Linux' netfilter, BSD's pf (used in OpenBSD, NetBSD, FreeBSD and MacOS X) as well as router's access-lists. It is similar to enterprise firewall management software. Complete fwbuilder's functionality is also available from the command line.
										
shorewall, a firewall configuration tool which provides support for IPsec as well as limited support for traffic shaping as well as the definition of the firewall rules. Configuration is done through a simple set of files that are used to generate the iptables rules.
										
bastille, this hardening application is described in [ChapterÂ 6, Automatic hardening of Debian systems](automatic-harden.en.md). One of the hardening steps that the administrator can configure is a definition of the allowed and disallowed network traffic that is used to generate a set of firewall rules that the system will execute on startup.
										

					Lots of other iptables frontends come with Debian; an extensive list comparing the different packages in Debian is maintained at the [http://wiki.debian.org/Firewalls](http://wiki.debian.org/Firewalls).
				
					Notice that some of the packages outlined previously will introduce firewalling scripts to be run when the system boots. Test them extensively before rebooting or you might find yourself locked from the box. If you mix different firewalling packages you can have undesired effects, usually, the firewalling script that runs last will be the one that configures the system (which might not be what you intend). Consult the package documentation and use either one of these setups.
				
					As mentioned before, some programs, like firestarter, guarddog and knetfilter, are administration GUIs using either GNOME or KDE (last two). These applications are much more user-oriented (i.e. for home users) than some of the other packages in the list which might be more administrator-oriented. Some of the programs mentioned before (like bastille) are focused at setting up firewall rules to protect the host they run in but are not necessarily designed to setup firewall rules for firewall hosts that protect a network (like shorewall or fwbuilder).
				
					There is yet another type of firewall application: application proxies. If you are looking into setting up an enterprise-level firewall that does packet filtering and provides a number of transparent proxies that can do fine-grain traffic analysis you should consider using zorp, which provides this in a single program. You can also manually setup this type of firewall host using the proxies available in Debian for different services like for DNS using bind (properly configured), dnsmasq, pdnsd or totd for FTP using frox or ftp-proxy, for X11 using xfwp, for IMAP using imapproxy, for mail using smtpd, or for POP3 using p3scan. For other protocols you can either use a generic TCP proxy like simpleproxy or a generic SOCKS proxy like dante-server, tsocks or socks4-server. Typically, you will also use a web caching system (like squid) and a web filtering system (like squidguard or dansguardian).
				

#### 5.14.3.2.Â Manual init.d configuration

					Another possibility is to manually configure your firewall rules through an init.d script that will run all the iptables commands. Take the following steps: 
					
								Review the script below and adapt it to your needs.
							
								Test the script and review the syslog messages to see which traffic is being dropped. If you are testing from the network you will want to either run the sample shell snippet to remove the firewall (if you don't type anything in 20 seconds) or you might want to comment out the default deny policy definitions (-P INPUT DROP and -P OUTPUT DROP) and check that the system will not drop any legitimate traffic.
							
								Move the script to /etc/init.d/myfirewall

								The below script takes advantage of Debian's use (since Squeeze) of dependency based boot sequencing. For more information see: [https://wiki.debian.org/LSBInitScripts/DependencyBasedBoot](https://wiki.debian.org/LSBInitScripts/DependencyBasedBoot) and [https://wiki.debian.org/LSBInitScripts](https://wiki.debian.org/LSBInitScripts). With the LSB headers set as they are in the script, insserv will automatically configure the system to start the firewall before any network is brought up, and stop the firewall after any network is brought down. 

```bash
# insserv myfirewall
```

					This is the sample firewall script: 

```bash
#!/bin/sh
### BEGIN INIT INFO
# Provides:          myfirewall
# Required-Start:    $local_fs
# Required-Stop:     $local_fs
# Default-Start:     S
# Default-Stop:      0 6
# X-Start-Before:    $network
# X-Stop-After:      $network
# Short-Description: My custom firewall.
### END INIT INFO
#
# Simple example firewall configuration.
#
# Caveats:
# - This configuration applies to all network interfaces
#   if you want to restrict this to only a given interface use
#   '-i INTERFACE' in the iptables calls.
# - Remote access for TCP/UDP services is granted to any host, 
#   you probably will want to restrict this using '--source'.
#
# chkconfig: 2345 9 91
# description: Activates/Deactivates the firewall at boot time
#
# You can test this script before applying with the following shell
# snippet, if you do not type anything in 10 seconds the firewall
# rules will be cleared.
#---------------------------------------------------------------
#  while true; do test=""; read  -t 20 -p "OK? " test ; \
#  [ -z "$test" ] && /etc/init.d/myfirewall clear ; done
#---------------------------------------------------------------

PATH=/bin:/sbin:/usr/bin:/usr/sbin

# Services that the system will offer to the network
TCP_SERVICES="22" # SSH only
UDP_SERVICES=""
# Services the system will use from the network
REMOTE_TCP_SERVICES="80" # web browsing
REMOTE_UDP_SERVICES="53" # DNS
# Network that will be used for remote mgmt
# (if undefined, no rules will be setup)
# NETWORK_MGMT=192.168.0.0/24
# If you want to setup a management network (i.e. you've uncommented
# the above line) you will need to define the SSH port as well (i.e.
# uncomment the below line.) Remember to remove the SSH port from the
# TCP_SERVICES string.
# SSH_PORT="22"

if ! [ -x /sbin/iptables ]; then  
    exit 0
fi

fw_start () {

  # Input traffic:
  /sbin/iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
  # Services
  if [ -n "$TCP_SERVICES" ] ; then
  for PORT in $TCP_SERVICES; do
    /sbin/iptables -A INPUT -p tcp --dport ${PORT} -j ACCEPT
  done
  fi
  if [ -n "$UDP_SERVICES" ] ; then
  for PORT in $UDP_SERVICES; do
    /sbin/iptables -A INPUT -p udp --dport ${PORT} -j ACCEPT
  done
  fi
  # Remote management
  if [ -n "$NETWORK_MGMT" ] ; then
    /sbin/iptables -A INPUT -p tcp --src ${NETWORK_MGMT} --dport ${SSH_PORT} -j ACCEPT
  fi
  # Remote testing
  /sbin/iptables -A INPUT -p icmp -j ACCEPT
  /sbin/iptables -A INPUT -i lo -j ACCEPT
  /sbin/iptables -P INPUT DROP
  /sbin/iptables -A INPUT -j LOG

  # Output:
  /sbin/iptables -A OUTPUT -j ACCEPT -o lo 
  /sbin/iptables -A OUTPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
  # ICMP is permitted:
  /sbin/iptables -A OUTPUT -p icmp -j ACCEPT
  # So are security package updates:
  # Note: You can hardcode the IP address here to prevent DNS spoofing
  # and to setup the rules even if DNS does not work but then you 
  # will not "see" IP changes for this service:
  /sbin/iptables -A OUTPUT -p tcp -d security.debian.org --dport 80 -j ACCEPT 
  # As well as the services we have defined:
  if [ -n "$REMOTE_TCP_SERVICES" ] ; then
  for PORT in $REMOTE_TCP_SERVICES; do
    /sbin/iptables -A OUTPUT -p tcp --dport ${PORT} -j ACCEPT
  done
  fi
  if [ -n "$REMOTE_UDP_SERVICES" ] ; then
  for PORT in $REMOTE_UDP_SERVICES; do
    /sbin/iptables -A OUTPUT -p udp --dport ${PORT} -j ACCEPT
  done
  fi
  # All other connections are registered in syslog
  /sbin/iptables -A OUTPUT -j LOG
  /sbin/iptables -A OUTPUT -j REJECT 
  /sbin/iptables -P OUTPUT DROP
  # Other network protections
  # (some will only work with some kernel versions)
  echo 1 > /proc/sys/net/ipv4/tcp_syncookies
  echo 0 > /proc/sys/net/ipv4/ip_forward 
  echo 1 > /proc/sys/net/ipv4/icmp_echo_ignore_broadcasts 
  echo 1 > /proc/sys/net/ipv4/conf/all/log_martians 
  echo 1 > /proc/sys/net/ipv4/ip_always_defrag
  echo 1 > /proc/sys/net/ipv4/icmp_ignore_bogus_error_responses
  echo 1 > /proc/sys/net/ipv4/conf/all/rp_filter
  echo 0 > /proc/sys/net/ipv4/conf/all/send_redirects
  echo 0 > /proc/sys/net/ipv4/conf/all/accept_source_route

}

fw_stop () {
  /sbin/iptables -F
  /sbin/iptables -t nat -F
  /sbin/iptables -t mangle -F
  /sbin/iptables -P INPUT DROP
  /sbin/iptables -P FORWARD DROP
  /sbin/iptables -P OUTPUT ACCEPT
}

fw_clear () {
  /sbin/iptables -F
  /sbin/iptables -t nat -F
  /sbin/iptables -t mangle -F
  /sbin/iptables -P INPUT ACCEPT
  /sbin/iptables -P FORWARD ACCEPT
  /sbin/iptables -P OUTPUT ACCEPT
}

case "$1" in
  start|restart)
    echo -n "Starting firewall.."
    fw_stop 
    fw_start
    echo "done."
    ;;
  stop)
    echo -n "Stopping firewall.."
    fw_stop
    echo "done."
    ;;
  clear)
    echo -n "Clearing firewall rules.."
    fw_clear
    echo "done."
    ;;
  *)
    echo "Usage: $0 {start|stop|restart|clear}"
    exit 1
    ;;
  esac
exit 0
```

					Instead of including all of the iptables rules in the init.d script you can use the iptables-restore program to restore the rules saved using iptables-save. In order to do this you need to setup your rules, save the ruleset under a static location (such as /etc/default/firewall)
				

#### 5.14.3.3.Â Configuring firewall rules through ifup

					You can use also the network configuration in /etc/network/interfaces to setup your firewall rules. For this you will need to: 
					
								Create your firewalling ruleset for when the interface is active.
							
								Save your ruleset with iptables-save to a file in /etc, for example /etc/iptables.up.rules

								Configure /etc/network/interfaces to use the configured ruleset: 

```bash
iface eth0 inet static
        address x.x.x.x
        [.. interface configuration ..]
        pre-up iptables-restore < /etc/iptables.up.rules
```

					You can optionally also setup a set of rules to be applied when the network interface is down creating a set of rules, saving it in /etc/iptables.down.rules and adding this directive to the interface configuration: 

```bash
post-down iptables-restore < /etc/iptables.down.rules
```

					For more advanced firewall configuration scripts through ifupdown you can use the hooks available to each interface as in the *.d/ directories called with run-parts (see run-parts(8) manual page).
				

#### 5.14.3.4.Â Testing your firewall configuration

					Testing your firewall configuration is as easy, and as dangerous, as just running your firewall script (or enabling the configuration you defined in your firewall configuration application). However, if you are not careful enough and you are configuring your firewall remotely (like through an SSH connection) you could lock yourself out.
				
					There are several ways to prevent this. One is running a script in a separate terminal that will remove the firewall configuration if you don't feed it input. An example of this is: 

```bash
$  while true; do test=""; read  -t 20 -p "OK? " test ; \
  [ -z "$test" ] && /etc/init.d/firewall clear ; done
```

					Another one is to introduce a backdoor in your system through an alternate mechanism that allows you to either clear the firewall system or punch a hole in it if something goes awry. For this you can use knockd and configure it so that a certain port connection attempt sequence will clear the firewall (or add a temporary rule). Even though the packets will be dropped by the firewall, since knockd binds to the interface and sees you will be able to work around the problem.
				
					Testing a firewall that is protecting an internal network is a different issue, you will want to look at some of the tools used for remote vulnerability assessment (see [SectionÂ 8.1, âRemote vulnerability assessment toolsâ](sec-tools.en.md#vuln-asses)) to probe the network from the outside in (or from any other direction) to test the effectiveness of the firewall configuation.
				[[43]](#idm2903)
				Available since the kernel version 2.4 (which was the default kernel in Debian 3.0). Previous kernel versions (2.2, available in even older Debian releases) used ipchains. The main difference between ipchains and iptables is that the latter is based on stateful packet inspection which provides for more secure (and easier to build) filtering configurations. Older (and now unsupported) Debian distributions using the 2.0 kernel series needed the appropriate kernel patch.
			[[44]](#idm2924)
								Unlike personal firewalls in other operating systems, Debian GNU/Linux does not (yet) provide firewall generation interfaces that can make rules limiting them per process or user. However, the iptables code can be configured to do this (see the owner module in the iptables(8) manual page).
							[Prev5.13.Â Securing RPC services](rpc.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 6.Â Automatic hardening of Debian systems](automatic-harden.en.md)