---
title: "9.5.2. The Configuration File"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.syslog.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.syslog.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.5.2. The Configuration File"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.administration-interfaces.en.md)The Debian Administrator's Handbook[Next](sect.inetd.en.md)

## 9.5.Â syslog System Events

### 9.5.1.Â Principle and Mechanism

				The rsyslogd daemon is responsible for collecting service messages coming from applications and the kernel, then dispatching them into log files (usually stored in the /var/log/ directory). It obeys the /etc/rsyslog.conf configuration file.
			
				Each log message is associated with an application subsystem (called âfacilityâ in the documentation):
			
auth and authpriv: for authentication;
					
cron: comes from task scheduling services, cron and atd;
					
daemon: affects a daemon without any special classification (DNS, NTP, etc.);
					
ftp: concerns the FTP server;
					
kern: message coming from the kernel;
					
lpr: comes from the printing subsystem;
					
mail: comes from the e-mail subsystem;
					
news: Usenet subsystem message (especially from an NNTP â Network News Transfer Protocol â server that manages newsgroups);
					
syslog: messages from the syslogd server, itself;
					
user: user messages (generic);
					
uucp: messages from the UUCP server (Unix to Unix Copy Program, an old protocol notably used to distribute e-mail messages);
					
local0 to local7: reserved for local use.
					
				Each message is also associated with a priority level. Here is the list in decreasing order:
			
emerg: âHelp!â There is an emergency, the system is probably unusable.
					
alert: hurry up, any delay can be dangerous, action must be taken immediately;
					
crit: conditions are critical;
					
err: error;
					
warn: warning (potential error);
					
notice: conditions are normal, but the message is important;
					
info: informative message;
					
debug: debugging message.
					

### 9.5.2.Â The Configuration File

				The syntax of the /etc/rsyslog.conf file is detailed in the rsyslog.conf(5) manual page, but there is also HTML documentation available in the rsyslog-doc package (/usr/share/doc/rsyslog-doc/html/index.html). The overall principle is to write âselectorâ and âactionâ pairs. The selector defines all relevant messages, and the action describes how to deal with them.
			

#### 9.5.2.1.Â Syntax of the Selector

					The selector is a semicolon-separated list of subsystem.priority pairs (example: auth.notice;mail.info). An asterisk may represent all subsystems or all priorities (examples: *.alert or mail.*). Several subsystems can be grouped, by separating them with a comma (example: auth,mail.info). The priority indicated also covers messages of equal or higher priority; thus auth.alert indicates the auth subsystem messages of alert or emerg priority. Prefixed with an exclamation point (!), it indicates the opposite, in other words the strictly lower priorities; auth.!notice, thus, indicates messages issued from auth, with info or debug priority. Prefixed with an equal sign (=), it corresponds to precisely and only the priority indicated (auth.=notice only concerns messages from auth with notice priority).
				
					Each element in the list on the selector overrides previous elements. It is thus possible to restrict a set or to exclude certain elements from it. For example, kern.info;kern.!err means messages from the kernel with priority between info and warn. The none priority indicates the empty set (no priorities), and may serve to exclude a subsystem from a set of messages. Thus, *.crit;kern.none indicates all the messages of priority equal to or higher than crit not coming from the kernel.
				

#### 9.5.2.2.Â Syntax of Actions

BACK TO BASICS The named pipe, a persistent pipe

					A named pipe is a particular type of file that operates like a traditional pipe (the pipe that you make with the â|â symbol on the command line), but via a file. This mechanism has the advantage of being able to relate two unrelated processes. Anything written to a named pipe blocks the process that writes until another process attempts to read the data written. This second process reads the data written by the first, which can then resume execution.
				
					Such a file is created with the mkfifo command.
				
					The various possible actions are:
				
							add the message to a file (example: /var/log/messages);
						
							send the message to a remote syslog server (example: @log.falcot.com);
						
							send the message to an existing named pipe (example: |/dev/xconsole);
						
							send the message to one or more users, if they are logged in (example: root,rhertzog);
						
							send the message to all logged in users (example: *);
						
							write the message in a text console (example: /dev/tty8).
						

SECURITY Forwarding logs

					It is a good idea to record the most important logs on a separate machine (perhaps dedicated for this purpose), since this will prevent any possible intruder from removing traces of their intrusion (unless, of course, they also compromise this other server). Furthermore, in the event of a major problem (such as a kernel crash), you have the logs available on another machine, which increases your chances of determining the sequence of events that caused the crash.
				
					To accept log messages sent by other machines, you must reconfigure rsyslog: in practice, it is sufficient to activate the ready-for-use entries in /etc/rsyslog.conf ($ModLoad imudp and $UDPServerRun 514).
				[Prev9.4.Â Administration Interfaces](sect.administration-interfaces.en.md)[Up](#)[Home](index.en.md)[Next9.6.Â The inetd Super-Server](sect.inetd.en.md)