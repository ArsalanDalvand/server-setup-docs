---
title: "7.2.4. Reporting a Bug When a Problem Is Too Difficult"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.common-procedures.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.common-procedures.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "7.2.4. Reporting a Bug When a Problem Is Too Difficult"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](solving-problems.en.md)The Debian Administrator's Handbook[Next](basic-configuration.en.md)

## 7.2.Â Common Procedures

			The purpose of this section is to present some general tips on certain operations that an administrator will frequently have to perform. These procedures will of course not cover every possible case in an exhaustive way, but they may serve as starting points for the more difficult cases.
		

DISCOVERY Documentation in other languages

			Often, documentation translated into a non-English language is available in a separate package with the name of the corresponding package, followed by -lang (where lang is the two-letter ISO code for the language).
		
			For instance, the debian-reference-fr package is the French version of the reference guides for Debian (initially written in English by Osamu Aoki), and the manpages-de package contains the German version of the manual pages about using GNU/Linux.
		

### 7.2.1.Â Configuring a Program

				When you want to configure an unknown package, you must proceed in stages. First, you should read what the package maintainer has documented. Reading /usr/share/doc/package/README.Debian will allow you to learn of specific provisions made to simplify the use of the software. It is sometimes essential in order to understand the differences from the original behavior of the program, as described in the general documentation, such as howtos. Sometimes this file also details the most common errors in order for you to avoid wasting time on common problems.
			
				Then, you should look at the software's official documentation â refer to [SectionÂ 7.1, âDocumentation Sourcesâ](solving-problems.en.md#sect.documentation-sources) to identify the various existing documentation sources. The dpkg -L package command gives a list of files included in the package; you can therefore quickly identify the available documentation (as well as the configuration files, located in /etc/). dpkg -s package displays the package meta-data and shows any possible recommended or suggested packages; in there, you can find documentation or a utility that will ease the configuration of the software.
			
				Finally, the configuration files are often self-documented by many explanatory comments detailing the various possible values for each configuration setting. So much so that it is sometimes enough to just choose a line to activate from among those available. In some cases, examples of configuration files are provided in the /usr/share/doc/package/examples/ directory. They may serve as a basis for your own configuration file.
			

DEBIAN POLICY Location of examples

				All examples must be installed in the /usr/share/doc/package/examples/ directory. This may be a configuration file, program source code (an example of the use of a library), or a data conversion script that the administrator can use in certain cases (such as to initialize a database). If the example is specific to a particular architecture, it should be installed in /usr/lib/package/examples/ and there should be a link pointing to that file in the /usr/share/doc/package/examples/ directory.
			

### 7.2.2.Â Monitoring What Daemons Are Doing

				Understanding what a daemon does is somewhat more complicated, since it does not interact directly with the administrator. To check that a daemon is actually working, you need to test it. For example, to check the Apache (web server) daemon, test it with an HTTP request.
			

BACK TO BASICS Daemon

				A daemon is a program that is not explicitly invoked by the user and that stays in the background, waiting for a certain condition to be met before performing a task. Many server programs are daemons, a term that explains that the letter âdâ is frequently present at the end of their name (sshd, smtpd, httpd, etc.).
			
				To allow such tests, each daemon generally records everything that it does, as well as any errors that it encounters, in what are called âlog filesâ or âsystem logsâ. Logs are stored in /var/log/ or one of its subdirectories. To know the precise name of a log file for each daemon, see its documentation. Note: a single test is not always sufficient if it does not cover all the possible usage cases; some problems only occur in particular circumstances.
			

TOOL The rsyslogd daemon

rsyslogd is special: it collects logs (internal system messages) that are sent to it by other programs. Each log entry is associated with a subsystem (e-mail, kernel, authentication, etc.) and a priority; rsyslogd processes these two pieces of information to decide on what to do. The log message may be recorded in various log files, and/or sent to an administration console. The details are defined in the /etc/rsyslog.conf configuration file (documented in the manual page of the same name provided in the rsyslog-doc package).
			
				Certain C functions, which are specialized in sending logs, simplify the use of the rsyslogd daemon. However some daemons manage their own log files (this is the case, for example, of samba, which implements Windows shares on Linux).
			
				Note that when systemd is in use, the logs are actually collected by systemd before being forwarded to rsyslogd. They are thus also available via systemd's journal and can be consulted with journalctl (see [SectionÂ 9.1.1, âThe systemd init systemâ](unix-services.en.md#sect.systemd) for details).
			
				As a preventive operation, the administrator should regularly read the most relevant server logs. They can thus diagnose problems before they are even reported by disgruntled users. Indeed users may sometimes wait for a problem to occur repeatedly over several days before reporting it. In many cases, there are specific tools to analyze the contents of the larger log files. In particular, such utilities exist for web servers (such as analog, awstats, awffull for Apache), FTP servers, proxy/cache servers, firewalls, e-mail servers, DNS servers, and even for print servers. Other tools, such as logcheck (a software discussed in [ChapterÂ 14, Security](security.en.md)), scan these files in search of alerts to be dealt with.
			

### 7.2.3.Â Asking for Help on a Mailing List

				If your various searches haven't helped you to get to the root of a problem, it is possible to get help from other, perhaps more experienced people. This is exactly the purpose of the [debian-user@lists.debian.org](mailto:debian-user@lists.debian.org) mailing list and its language specific siblings [debian-user-lang@lists.debian.org](mailto:debian-user-lang@lists.debian.org). As with any community, it has rules that need to be followed. Before asking any question, you should check that your problem isn't already covered by recent discussions on the list or by any official documentation. âÂ [https://wiki.debian.org/DebianMailingLists](https://wiki.debian.org/DebianMailingLists) âÂ [https://lists.debian.org/debian-user/](https://lists.debian.org/debian-user/) âÂ [https://lists.debian.org/users.html](https://lists.debian.org/users.html)

BACK TO BASICS Netiquette applies

				In general, for all correspondence on e-mail lists, the rules of Netiquette should be followed. This term refers to a set of common sense rules, from common courtesy to mistakes that should be avoided. âÂ [https://tools.ietf.org/html/rfc1855](https://tools.ietf.org/html/rfc1855)

				Furthermore, for any communication channel managed by the Debian project, you are bound by the Debian Code of Conduct: âÂ [https://www.debian.org/code_of_conduct](https://www.debian.org/code_of_conduct)

				Once those two conditions are met, you can think of describing your problem to the mailing list. Include as much relevant information as possible: various tests conducted, documentation consulted, how you attempted to diagnose the problem, the packages concerned or those that may be involved, etc. Check the Debian Bug Tracking System (BTS, described in sidebar [SectionÂ 1.3.2.1, âReporting bugsâ](sect.debian-internals.en.md#sect.bts)) for similar problems, and mention the results of that search, providing links to bugs found. BTS starts on: âÂ [https://bugs.debian.org/](https://bugs.debian.org/)

				The more courteous and precise you have been, the greater your chances are of getting an answer, or, at least, some elements of response. If you receive relevant information by private e-mail, think of summarizing this information publicly so that others can benefit. This also allows the list's archives, searched through various search engines, to show the resolution for others who may have the same question.
			

### 7.2.4.Â Reporting a Bug When a Problem Is Too Difficult

				If all of your efforts to resolve a problem fail, it is possible that a resolution is not your responsibility, and that the problem is due to a bug in the program. In this case, the proper procedure is to report the bug to Debian or directly to the upstream developers. To do this, isolate the problem as much as possible and create a minimal test situation in which it can be reproduced. If you know which program is the apparent cause of the problem, you can find its corresponding package using the command, dpkg -S file_in_question. Check the Bug Tracking System (https://bugs.debian.org/package) to ensure that the bug has not already been reported. You can then send your own bug report, using the reportbug command, including as much information as possible, especially a complete description of those minimal test cases that will allow anyone to recreate the bug.
			
				The elements of this chapter are a means of effectively resolving issues that the following chapters may bring about. Use them as often as necessary!
			[PrevChapterÂ 7.Â Solving Problems and Finding Relevant ...](solving-problems.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 8.Â Basic Configuration: Network, Accounts...](basic-configuration.en.md)