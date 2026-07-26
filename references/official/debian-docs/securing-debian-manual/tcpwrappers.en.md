---
title: "4.12. Using tcpwrappers"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/tcpwrappers.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/tcpwrappers.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.12. Using tcpwrappers"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch04s11.en.md)Securing Debian Manual[Next](log-alerts.en.md)

## 4.12.Â Using tcpwrappers

			TCP wrappers were developed when there were no real packet filters available and access control was needed. Nevertheless, they're still very interesting and useful. The TCP wrappers allow you to allow or deny a service for a host or a domain and define a default allow or deny rule (all performed on the application level). If you want more information take a look at hosts_access(5) manual page.
		
			Many services installed in Debian are either: 
			
						launched through the tcpwrapper service (tcpd)
					
						compiled with libwrapper support built-in.
					

			On the one hand, for services configured in /etc/inetd.conf (this includes telnet, ftp, netbios, swat and finger) you will see that the configuration file executes /usr/sbin/tcpd first. On the other hand, even if a service is not launched by the inetd superdaemon, support for the tcp wrappers rules can be compiled into it. Services compiled with tcp wrappers in Debian include ssh, portmap, in.talk, rpc.statd, rpc.mountd, gdm, oaf (the GNOME activator daemon), nessus and many others.
		
			To see which packages use tcpwrappers [[25]](#ftn.idm1707) try: 

```bash
$ apt-cache rdepends libwrap0
```

			Take this into account when running tcpdchk (a very useful TCP wrappers config file rule and syntax checker). When you add stand-alone services (that are directly linked with the wrapper library) into the hosts.deny and hosts.allow files, tcpdchk will warn you that it is not able to find the mentioned services since it only looks for them in /etc/inetd.conf (the manpage is not totally accurate here).
		
			Now, here comes a small trick, and probably the smallest intrusion detection system available. In general, you should have a decent firewall policy as a first line, and tcp wrappers as the second line of defense. One little trick is to set up a SPAWN [[26]](#ftn.idm1719) command in /etc/hosts.deny that sends mail to root whenever a denied service triggers wrappers: 

```bash
ALL: ALL: SPAWN ( \
    echo -e "\n\
    TCP Wrappers\: Connection refused\n\
    By\: $(uname -n)\n\
    Process\: %d (pid %p)\n\
    User\: %u\n\
    Host\: %c\n\
    Date\: $(date)\n\
  " | /usr/bin/mail -s "Connection to %d blocked" root) &
```

Beware: The above printed example is open to a DoS attack by making many connections in a short period of time. Many emails mean a lot of file I/O by sending only a few packets.
		[[25]](#idm1707)
				On older Debian releases you might need to do this: 

```bash
$ apt-cache showpkg libwrap0 | egrep '^[[:space:]]' | sort -u | \
        sed 's/,libwrap0$//;s/^[[:space:]]\+//'
```

[[26]](#idm1719)
				be sure to use uppercase here since spawn will not work
			[Prev4.11.Â Providing secure user access](ch04s11.en.md)[Up](#)[Home](index.en.md)[Next4.13.Â The importance of logs and alerts](log-alerts.en.md)