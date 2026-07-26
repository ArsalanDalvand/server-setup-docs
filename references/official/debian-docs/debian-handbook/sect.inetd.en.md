---
title: "9.6. TheinetdSuper-Server"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.inetd.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.inetd.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.6. TheinetdSuper-Server"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.syslog.en.md)The Debian Administrator's Handbook[Next](sect.task-scheduling-cron-atd.en.md)

## 9.6.Â The inetd Super-Server

			Inetd (often called âInternet super-serverâ) is a server of servers. It executes rarely used servers on demand, so that they do not have to run continuously.
		
			The /etc/inetd.conf file lists these servers and their usual ports. The inetd command listens to all of them; when it detects a connection to any such port, it executes the corresponding server program.
		

DEBIAN POLICY Register a server in /etc/inetd.conf

			Packages frequently want to register a new server in the /etc/inetd.conf file, but Debian Policy prohibits any package from modifying a configuration file that it doesn't own. This is why the update-inetd script (in the package with the same name) was created: It manages the configuration file, and other packages can thus use it to register a new server to the super-server's configuration.
		
			Each significant line of the /etc/inetd.conf file describes a server through seven fields (separated by spaces):
		
					The TCP or UDP port number, or the service name (which is mapped to a standard port number with the information contained in the /etc/services file).
				
					The socket type: stream for a TCP connection, dgram for UDP datagrams.
				
					The protocol: tcp, tcp6, udp, or udp6.
				
					The options: two possible values: wait or nowait, to tell inetd whether it should wait or not for the end of the launched process before accepting another connection. For TCP connections, easily multiplexable, you can usually use nowait. For programs responding over UDP, you should use nowait only if the server is capable of managing several connections in parallel. You can suffix this field with a period, followed by the maximum number of connections authorized per minute (the default limit is 256).
				
					The user name of the user under whose identity the server will run. Optionally one can add the group too via user.group syntax.
				
					The full path to the server program to execute.
				
					The arguments: this is a complete list of the program's arguments, including its own name (argv[0] in C).
				
			The following example illustrates some use-cases after installing talkd, nullidentd (ident-server), and fingerd:
		

ExampleÂ 9.1.Â Excerpt from /etc/inetd.conf

```bash
#:BSD: Shell, login, exec and talk are BSD protocols.
talk   dgram   udp     wait    nobody.tty   /usr/sbin/in.talkd      in.talkd
ntalk  dgram   udp     wait    nobody.tty   /usr/sbin/in.ntalkd     in.ntalkd

#:INFO: Info services
ident  stream  tcp     nowait  nobody       /usr/sbin/nullidentd    nullidentd
finger stream  tcp     nowait  nobody       /usr/sbin/tcpd          /usr/sbin/in.fingerd
```

			The tcpd program is frequently used in the /etc/inetd.conf file. It allows limiting incoming connections by applying access control rules, documented in the hosts_access(5) manual page, and which are configured in the /etc/hosts.allow and /etc/hosts.deny files. Once it has been determined that the connection is authorized, tcpd executes the real server (like in.fingerd in our example). It is worth noting that tcpd relies on the name under which it was invoked (that is the first argument, argv[0]) to identify the real program to run. So you should not start the arguments list with tcpd but with the program that must be wrapped.
		

COMMUNITY Wietse Venema

			Wietse Venema, whose expertise in security has made him a renowned programmer, is the author of the tcpd program. He is also the main creator of Postfix, the modular e-mail server (SMTP, Simple Mail Transfer Protocol), designed to be safer and more reliable than sendmail, which features a long history of security vulnerabilities. We will have a closer look at this mail server in [SectionÂ 11.1, âMail Serverâ](network-services.en.md#sect.smtp-mail-server).
		

ALTERNATIVE Other inetd commands

			While Debian installs openbsd-inetd by default, there is no lack of alternatives: we can mention inetutils-inetd, rlinetd, and xinetd, which all provide the virtual package inet-superserver.
		
			Most of these alternatives share the same configuration file /etc/inetd.conf.
		
			This last incarnation of a super-server however, offers different syntax and very interesting possibilities. Most notably, its configuration can be split into several files (stored, of course, in the /etc/xinetd.d/ directory), which can make an administrator's life easier. It is considered to be more powerful, but also more complex.
		
			Last but not least, it is even possible to emulate inetd's behavior with systemd's socket-activation mechanism (see [SectionÂ 9.1.1, âThe systemd init systemâ](unix-services.en.md#sect.systemd)).
		[Prev9.5.Â syslog System Events](sect.syslog.en.md)[Up](#)[Home](index.en.md)[Next9.7.Â Scheduling Tasks with cron and atd](sect.task-scheduling-cron-atd.en.md)