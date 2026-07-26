---
title: "10.9.3. Sniffers:tcpdumpandwireshark"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.network-diagnosis-tools.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.network-diagnosis-tools.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "10.9.3. Sniffers:tcpdumpandwireshark"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.dhcp.en.md)The Debian Administrator's Handbook[Next](network-services.en.md)

## 10.9.Â Network Diagnosis Tools

			When a network application does not run as expected, it is important to be able to look under the hood. Even when everything seems to run smoothly, running a network diagnosis can help ensure everything is working as it should. Several diagnosis tools exists for this purpose; each one operates on a different level. It would go beyond the scope of this book to discuss all tools, so we will focus on the more well-known and commonly used tools in the following sections.
		

### 10.9.1.Â Local Diagnosis: netstat

				Let's first mention the netstat command (in the net-tools package); it displays an instant summary of a machine's network activity. When invoked with no argument, this command lists all open connections; this list can be very verbose since it includes many Unix-domain sockets (widely used by daemons) which do not involve the network at all (for example, dbus communication, X11 traffic, and communications between virtual filesystems and the desktop).
			
				Common invocations therefore use options that alter netstat's behavior. The most frequently used options include:
			
-t, which filters the results to only include TCP connections;
					
-u, which works similarly for UDP connections; these options are not mutually exclusive, and one of them is enough to stop displaying Unix-domain connections;
					
-a, to also list listening sockets (waiting for incoming connections);
					
-n, to display the results numerically: IP addresses (no DNS resolution), port numbers (no aliases as defined in /etc/services) and user ids (no login names);
					
-p, to list the processes involved; this option is only useful when netstat is run as root, since normal users will only see their own processes;
					
-c, to continuously refresh the list of connections.
					
				Other options, documented in the netstat(8) manual page, provide an even finer control over the displayed results. In practice, the first five options are so often used together that systems and network administrators practically acquired netstat -tupan as a reflex. Typical results, on a lightly loaded machine, may look like the following:
			

```bash
# netstat -tupan
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      397/rpcbind     
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      431/sshd        
tcp        0      0 0.0.0.0:36568           0.0.0.0:*               LISTEN      407/rpc.statd   
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      762/exim4       
tcp        0    272 192.168.1.242:22        192.168.1.129:44452     ESTABLISHED 1172/sshd: roland [
tcp6       0      0 :::111                  :::*                    LISTEN      397/rpcbind     
tcp6       0      0 :::22                   :::*                    LISTEN      431/sshd        
tcp6       0      0 ::1:25                  :::*                    LISTEN      762/exim4       
tcp6       0      0 :::35210                :::*                    LISTEN      407/rpc.statd   
udp        0      0 0.0.0.0:39376           0.0.0.0:*                           916/dhclient    
udp        0      0 0.0.0.0:996             0.0.0.0:*                           397/rpcbind     
udp        0      0 127.0.0.1:1007          0.0.0.0:*                           407/rpc.statd   
udp        0      0 0.0.0.0:68              0.0.0.0:*                           916/dhclient    
udp        0      0 0.0.0.0:48720           0.0.0.0:*                           451/avahi-daemon: r
udp        0      0 0.0.0.0:111             0.0.0.0:*                           397/rpcbind     
udp        0      0 192.168.1.242:123       0.0.0.0:*                           539/ntpd        
udp        0      0 127.0.0.1:123           0.0.0.0:*                           539/ntpd        
udp        0      0 0.0.0.0:123             0.0.0.0:*                           539/ntpd        
udp        0      0 0.0.0.0:5353            0.0.0.0:*                           451/avahi-daemon: r
udp        0      0 0.0.0.0:39172           0.0.0.0:*                           407/rpc.statd   
udp6       0      0 :::996                  :::*                                397/rpcbind     
udp6       0      0 :::34277                :::*                                407/rpc.statd   
udp6       0      0 :::54852                :::*                                916/dhclient    
udp6       0      0 :::111                  :::*                                397/rpcbind     
udp6       0      0 :::38007                :::*                                451/avahi-daemon: r
udp6       0      0 fe80::5054:ff:fe99::123 :::*                                539/ntpd        
udp6       0      0 2001:bc8:3a7e:210:a:123 :::*                                539/ntpd        
udp6       0      0 2001:bc8:3a7e:210:5:123 :::*                                539/ntpd        
udp6       0      0 ::1:123                 :::*                                539/ntpd        
udp6       0      0 :::123                  :::*                                539/ntpd        
udp6       0      0 :::5353                 :::*                                451/avahi-daemon: r
```

				As expected, this lists established connections, two SSH connections in this case, and applications waiting for incoming connections (listed as LISTEN), notably the Exim4 email server listening on port 25.
			

### 10.9.2.Â Remote Diagnosis: nmap

nmap (in the similarly-named package) is, in a way, the remote equivalent for netstat. It can scan a set of âwell-knownâ ports for one or several remote servers, and list the ports where an application is found to answer to incoming connections. Furthermore, nmap is able to identify some of these applications, sometimes even their version number. The counterpart of this tool is that, since it runs remotely, it cannot provide information on processes or users; however, it can operate on several targets at once.
			
				A typical nmap invocation only uses the -A option (so that nmap attempts to identify the versions of the server software it finds) followed by one or more IP addresses or DNS names of machines to scan. Again, many more options exist to finely control the behavior of nmap; please refer to the documentation in the nmap(1) manual page.
			

```bash
# nmap debian
Starting Nmap 7.80 ( https://nmap.org ) at 2022-02-22 20:58 CET
Nmap scan report for debian (192.168.122.57)
Host is up (0.000087s latency).
Not shown: 996 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
79/tcp  open  finger
80/tcp  open  http
113/tcp open  ident

Nmap done: 1 IP address (1 host up) scanned in 0.07 seconds
# nmap -A localhost
nmap -A localhost
Starting Nmap 7.80 ( https://nmap.org ) at 2022-02-22 20:56 CET
Stats: 0:01:16 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 83.33% done; ETC: 20:57 (0:00:15 remaining)
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000086s latency).
Other addresses for localhost (not scanned): ::1
Not shown: 994 closed ports
PORT    STATE SERVICE VERSION
22/tcp  open  ssh     OpenSSH 8.4p1 Debian 5 (protocol 2.0)
|_auth-owners: foobar
25/tcp  open  smtp    Postfix smtpd
|_auth-owners: foobar
|_smtp-commands: debian, PIPELINING, SIZE 10240000, VRFY, ETRN, STARTTLS, ENHANCEDSTATUSCODES, 8BITMIME, DSN, SMTPUTF8, CHUNKING, 
| ssl-cert: Subject: commonName=debian
| Subject Alternative Name: DNS:debian
| Not valid before: 2022-02-22T14:48:42
|_Not valid after:  2032-02-20T14:48:42
|_ssl-date: TLS randomness does not represent time
79/tcp  open  finger?
|_auth-owners: foobar
|_finger: ERROR: Script execution failed (use -d to debug)
80/tcp  open  http    Apache httpd 2.4.52 ((Debian))
|_auth-owners: foobar
|_http-server-header: Apache/2.4.52 (Debian)
|_http-title: Apache2 Debian Default Page: It works
113/tcp open  ident   Liedentd (Claimed user: foobar)
|_auth-owners: foobar
631/tcp open  ipp     CUPS 2.3
|_auth-owners: foobar
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: CUPS/2.3 IPP/2.1
|_http-title: Home - CUPS 2.3.3op2
Service Info: Host:  debian; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 87.91 seconds
```

				As expected, e.g. the SSH, Apache and Postfix applications are listed. Note that not all applications listen on all IP addresses; since Postfix is only accessible on the lo loopback interface, it only appears during an analysis of localhost and not when scanning debian (which maps to the enp1s0 interface on the same machine).
			

### 10.9.3.Â Sniffers: tcpdump and wireshark

				Sometimes, one needs to look at what actually goes on the wire, packet by packet. These cases call for a âframe analyzerâ, more widely known as a sniffer. Such a tool observes all the packets that reach a given network interface, and displays them in a user-friendly way.
			
				The venerable tool in this domain is tcpdump, available as a standard tool on a wide range of platforms. It allows many kinds of network traffic capture, but the representation of this traffic stays rather obscure. We will therefore not describe it in further detail.
			
				A more recent (and more modern) tool, wireshark (in the wireshark package), has become the new reference in network traffic analysis due to its many decoding modules that allow for a simplified analysis of the captured packets. The packets are displayed graphically with an organization based on the protocol layers. This allows a user to visualize all protocols involved in a packet. For example, given a packet containing an HTTP request, wireshark displays, separately, the information concerning the physical layer, the Ethernet layer, the IP packet information, the TCP connection parameters, and finally the HTTP request itself.
			

FigureÂ 10.1.Â The wireshark network traffic analyzer

				In our example, the packets traveling over SSH are filtered out (with the !tcp.port == 22 filter). The packet currently displayed was developed at the transport layer of the SSHv2 protocol.
			

TIP wireshark with no graphical interface: tshark

				When one cannot run a graphical interface, or does not wish to do so for whatever reason, a text-only version of wireshark also exists under the name tshark (in a separate tshark package). Most of the capture and decoding features are still available, but the lack of a graphical interface necessarily limits the interactions with the program (filtering packets after they've been captured, tracking of a given TCP connection, and so on). It can still be used as a first approach. If further manipulations are intended and require the graphical interface, the packets can be saved to a file and this file can be loaded into a graphical wireshark running on another machine.
			[Prev10.8.Â DHCP](sect.dhcp.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 11.Â Network Services: Postfix, Apache, NF...](network-services.en.md)