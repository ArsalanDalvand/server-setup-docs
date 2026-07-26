---
title: "5.10.1. Making chrooted environments automatically"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/chroot.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/chroot.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "5.10.1. Making chrooted environments automatically"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch05s09.en.md)Securing Debian Manual[Next](ch05s11.en.md)

## 5.10.Â General chroot and suid paranoia

chroot is one of the most powerful possibilities to restrict a daemon or a user or another service. Just imagine a jail around your target, which the target cannot escape from (normally, but there are still a lot of conditions that allow one to escape out of such a jail). You can eventually create a modified root environment for the user or service you do not trust. This can use quite a bit of disk space as you need to copy all needed executables, as well as libraries, into the jail. But then, even if the user does something malicious, the scope of the damage is limited to the jail. 
		
			Many services running as daemons could benefit from this sort of arrangement. The daemons that you install with your Debian distribution will not come, however, chrooted[[42]](#ftn.idm2809) per default.
		
			This includes: name servers (such as bind), web servers (such as apache), mail servers (such as sendmail) and ftp servers (such as wu-ftpd). It is probably fair to say that the complexity of BIND is the reason why it has been exposed to a lot of attacks in recent years (see [SectionÂ 5.7, âSecuring BINDâ](sec-bind.en.md)).
		
			However, Debian does provide some software that can help set up chroot environments. See [SectionÂ 5.10.1, âMaking chrooted environments automaticallyâ](chroot.en.md#auto-chroot).
		
			Anyway, if you run any service on your system, you should consider running them as secure as possible. This includes: revoking root privileges, running in a restricted environment (such as a chroot jail) or replacing them with a more secure equivalent.
		
			However, be forewarned that a chroot jail can be broken if the user running in it is the superuser. So, you need to make the service run as a non-privileged user. By limiting its environment you are limiting the world readable/executable files the service can access, thus, you limit the possibilities of a privilege escalation by use of local system security vulnerabilities. Even in this situation you cannot be completely sure that there is no way for a clever attacker to somehow break out of the jail. Using only server programs which have a reputation for being secure is a good additional safety measure. Even minuscule holes like open file handles can be used by a skilled attacker for breaking into the system. After all, chroot was not designed as a security tool but as a testing tool.
		

### 5.10.1.Â Making chrooted environments automatically

				There are several programs to chroot automatically servers and services. Debian currently (accepted in May 2002) provides Wietse Venema's chrootuid in the chrootuid package, as well as compartment and makejail. These programs can be used to set up a restricted environment for executing any program (chrootuid enables you to even run it as a restricted user).
			
				Some of these tools can be used to set up the chroot environment easily. The makejail program for example, can create and update a chroot jail with short configuration files (it provides sample configuration files for bind, apache, postgresql and mysql). It attempts to guess and install into the jail all files required by the daemon using strace, stat and Debian's package dependencies. More information at [http://www.floc.net/makejail/](http://www.floc.net/makejail/). Jailer is a similar tool which can be retrieved from [http://www.balabit.hu/downloads/jailer/](http://www.balabit.hu/downloads/jailer/) and is also available as a Debian package.
			[[42]](#idm2809)
				It does try to run them under minimum priviledge which includes running daemons with their own users instead of having them run as root.
			[Prev5.9.Â Securing finger](ch05s09.en.md)[Up](#)[Home](index.en.md)[Next5.11.Â General cleartext password paranoia](ch05s11.en.md)