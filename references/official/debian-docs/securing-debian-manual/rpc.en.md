---
title: "5.13.2. Limiting access to RPC services"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/rpc.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/rpc.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "5.13.2. Limiting access to RPC services"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch05s12.en.md)Securing Debian Manual[Next](firewall-setup.en.md)

## 5.13.Â Securing RPC services

			You should disable RPC if you do not need it.
		
			Remote Procedure Call (RPC) is a protocol that programs can use to request services from other programs located on different computers. The portmap service controls RPC services by mapping RPC program numbers into DARPA protocol port numbers; it must be running in order to make RPC calls.
		
			RPC-based services have had a bad record of security holes, although the portmapper itself hasn't (but still provides information to a remote attacker). Notice that some of the DDoS (distributed denial of service) attacks use RPC exploits to get into the system and act as a so called agent/handler.
		
			You only need RPC if you are using an RPC-based service. The most common RPC-based services are NFS (Network File System) and NIS (Network Information System). See the previous section for more information about NIS. The File Alteration Monitor (FAM) provided by the package fam is also an RPC service, and thus depends on portmap.
		
			NFS services are quite important in some networks. If that is the case for you, then you will need to find a balance of security and usability for your network (you can read more about NFS security in the [http://www.tldp.org/HOWTO/NFS-HOWTO.html](http://www.tldp.org/HOWTO/NFS-HOWTO.html) (/usr/share/doc/HOWTO/en-txt/NFS-HOWTO.txt.gz)).
		

### 5.13.1.Â Disabling RPC services completely

				Disabling portmap is quite simple. There are several different methods. The simplest one in a Debian 3.0 system and later releases is to uninstall the portmap package. If you are running an older Debian version you will have to disable the service as seen in [SectionÂ 3.5.1, âDisabling daemon servicesâ](ch03s05.en.md#disableserv), because the program is part of the netbase package (which cannot be de-installed without breaking the system).
			
				Notice that some desktop environments (notably, GNOME) use RPC services and need the portmapper for some of the file management features. If this is your case, you can limit the access to RPC services as described below.
			

### 5.13.2.Â Limiting access to RPC services

				Unfortunately, in some cases removing RPC services from the system is not an option. Some local desktop services (notably SGI's fam) are RPC based and thus need a local portmapper. This means that under some situations, users installing a desktop environment (like GNOME) will install the portmapper too.
			
				There are several ways to limit access to the portmapper and to RPC services: 
				
							Block access to the ports used by these services with a local firewall (see [SectionÂ 5.14, âAdding firewall capabilitiesâ](firewall-setup.en.md)).
						
							Block access to these services using tcp wrappers, since the portmapper (and some RPC services) are compiled with libwrap (see [SectionÂ 4.12, âUsing tcpwrappersâ](tcpwrappers.en.md)). This means that you can block access to them through the hosts.allow and hosts.deny tcp wrappers configuration.
						
							Since version 5-5, the portmap package can be configured to listen only on the loopback interface. To do this, modify /etc/default/portmap, uncomment the following line: #OPTIONS="-i 127.0.0.1" and restart the portmapper. This is sufficient to allow local RPC services to work while at the same time prevents remote systems from accessing them (see, however, [SectionÂ 4.18.5, âDisabling weak-end hosts issuesâ](network-secure.en.md#limit-bindaddr)).
						
[Prev5.12.Â Disabling NIS](ch05s12.en.md)[Up](#)[Home](index.en.md)[Next5.14.Â Adding firewall capabilities](firewall-setup.en.md)