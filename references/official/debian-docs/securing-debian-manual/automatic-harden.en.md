---
title: "6.1. Harden"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/automatic-harden.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/automatic-harden.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "6.1. Harden"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](firewall-setup.en.md)Securing Debian Manual[Next](ch06s02.en.md)

# ChapterÂ 6.Â Automatic hardening of Debian systems

[6.1. Harden](automatic-harden.en.md#idm3101)[6.2. Bastille Linux](ch06s02.en.md)
		After reading through all the information in the previous chapters you might be wondering "I have to do quite a lot of things in order to harden my system, couldn't these things be automated?". The answer is yes, but be careful with automated tools. Some people believe, that a hardening tool does not eliminate the need for good administration. So do not be fooled to think that you can automate the whole process and will fix all the related issues. Security is an ever-ongoing process in which the administrator must participate and cannot just stand away and let the tools do all the work since no single tool can cope with all the possible security policy implementations, all the attacks and all the environments.
	
		Since woody (Debian 3.0) there are two specific packages that are useful for security hardening. The harden package which takes an approach based on the package dependencies to quickly install valuable security packages and remove those with flaws, configuration of the packages must be done by the administrator. The bastille package that implements a given security policy on the local system based on previous configuration by the administrator (the building of the configuration can be a guided process done with simple yes/no questions).
	

## 6.1.Â Harden

			The harden package tries to make it more easy to install and administer hosts that need good security. This package should be used by people that want some quick help to enhance the security of the system.  It automatically installs some tools that should enhance security in some way: intrusion detection tools, security analysis tools, etc. Harden installs the following virtual packages (i.e. no contents, just dependencies or recommendations on others): 
			
harden-tools: tools to enhance system security (integrity checkers, intrusion detection, kernel patches...)
					
harden-environment: helps configure a hardened environment (currently empty).
					
harden-servers: removes servers considered insecure for some reason.
					
harden-clients: removes clients considered insecure for some reason.
					
harden-remoteaudit: tools to remotely audit a system.
					
harden-nids: helps to install a network intrusion detection system.
					
harden-surveillance: helps to install tools for monitoring of networks and services.
					

			Useful packages which are not a dependence: 
			
harden-doc: provides this same manual and other security-related documentation packages.
					
harden-development: development tools for creating more secure programs.
					

			Be careful because if you have software you need (and which you do not wish to uninstall for some reason) and it conflicts with some of the packages above you might not be able to fully use harden. The harden packages do not (directly) do a thing. They do have, however, intentional package conflicts with known non-secure packages. This way, the Debian packaging system will not approve the installation of these packages. For example, when you try to install a telnet daemon with harden-servers, apt will say: 

```bash
# apt-get install telnetd 
The following packages will be REMOVED:
  harden-servers
The following NEW packages will be installed:
  telnetd 
Do you want to continue? [Y/n]
```

			This should set off some warnings in the administrator head, who should reconsider his actions.
		[Prev5.14.Â Adding firewall capabilities](firewall-setup.en.md)[Up](#)[Home](index.en.md)[Next6.2.Â Bastille Linux](ch06s02.en.md)