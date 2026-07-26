---
title: "B.3. Setting up a stand-alone IDS"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/snort-box.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/snort-box.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "B.3. Setting up a stand-alone IDS"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](checklist.en.md)Securing Debian Manual[Next](bridge-fw.en.md)

## B.3.Â Setting up a stand-alone IDS

			You can easily set up a dedicated Debian system as a stand-alone Intrusion Detection System using snort and a web-based interface to analyse the intrusion detection alerts: 
			
						Install a base Debian system and select no additional packages.
					
						Install one of the Snort versions with database support and configure the IDS to log alerts into the database.
					
						Download and install BASE (Basic Analysis and Security Engine), or ACID (Analysis Console for Intrusion Databases). Configure it to use the same database than Snort.
					
						Download and install the necessary packages[[76]](#ftn.idm6314).
					

			BASE is currently packaged for Debian in acidbase and ACID is packaged as acidlab[[77]](#ftn.idm6319). Both provide a graphical WWW interface to Snort's output.
		
			Besides the base installation you will also need a web server (such as apache), a PHP interpreter and a relational database (such postgresql or mysql) where Snort will store its alerts.
		
			This system should be set up with at least two interfaces: one interface connected to a management LAN (for accessing the results and maintaining the system), and one interface with no IP address attached to the network segment being analyzed. You should configure the web server to listen only on the interface connected to the management LAN.
		
			You should configure both interfaces in the standard Debian /etc/network/interfaces configuration file. One (the management LAN) address can be configured as you would normally do. The other interface needs to be configured so that it is started up when the system boots, but with no interface address. You can use the following interface definition: 

```bash
auto eth0
iface eth0 inet manual
      up ifconfig $IFACE 0.0.0.0 up
      up ip link set $IFACE promisc on
      down ip link set $IFACE promisc off
      down ifconfig $IFACE down
```

			The above configures an interface to read all the traffic on the network in a stealth-type configuration. This prevents the NIDS system to be a direct target in a hostile network since the sensors have no IP address on the network. Notice, however, that there have been known bugs over time in sensors part of NIDS (for example see [https://lists.debian.org/debian-security-announce/2003/msg00087.html](https://lists.debian.org/debian-security-announce/2003/msg00087.html) related to Snort) and remote buffer overflows might even be triggered by network packet processing.
		
			You might also want to read the [http://www.faqs.org/docs/Linux-HOWTO/Snort-Statistics-HOWTO.html](http://www.faqs.org/docs/Linux-HOWTO/Snort-Statistics-HOWTO.html) and the documentation available at the [https://www.snort.org/#documents](https://www.snort.org/#documents).
		[[76]](#idm6314)
							Typically the needed packages will be installed through the dependencies
						[[77]](#idm6319)
				It can also be downloaded from [http://www.cert.org/kb/acid/](http://www.cert.org/kb/acid/), [http://acidlab.sourceforge.net](http://acidlab.sourceforge.net) or [http://www.andrew.cmu.edu/~rdanyliw/snort/](http://www.andrew.cmu.edu/~rdanyliw/snort/).
			[PrevB.2.Â Configuration checklist](checklist.en.md)[Up](#)[Home](index.en.md)[NextB.4.Â Setting up a bridge firewall](bridge-fw.en.md)