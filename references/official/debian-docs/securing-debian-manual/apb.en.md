---
title: "B.1. The hardening process step by step"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/apb.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/apb.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "B.1. The hardening process step by step"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](changelog_history.en.md)Securing Debian Manual[Next](checklist.en.md)

# AppendixÂ B.Â Appendix

## B.1.Â The hardening process step by step

			Below is a post-installation, step-by-step procedure for hardening a Debian 2.2 GNU/Linux system. This is one possible approach to such a procedure and is oriented toward the hardening of network services. It is included to show the entire process you might use during configuration. Also, see [SectionÂ B.2, âConfiguration checklistâ](checklist.en.md).
		

						Install the system, taking into account the information regarding partitioning included earlier in this document. After base installation, go into custom install. Do not select task packages.
					
						Using dselect, remove all unneeded but selected packages before doing [I]nstall. Keep the bare minimum of packages for the system.
					
						Update all software from the latest packages available at security.debian.org as explained previously in [SectionÂ 4.2, âExecute a security updateâ](security-update.en.md).
					
						Implement the suggestions presented in this manual regarding user quotas, login definitions and lilo

						Make a list of services currently running on your system. Try: 

```bash
$ ps aux
  $ netstat -pn -l -A inet 
  # /usr/sbin/lsof -i | grep LISTEN
```

						 You will need to install lsof-2.2 for the third command to work (run it as root). You should be aware that lsof can translate the word LISTEN to your locale settings.
					
						In order to remove unnecessary services, first determine what package provides the service and how it is started. This can be accomplished by checking the program that listens in the socket. The following shell script, which uses the programs lsof and dpkg, does just that: 

```bash
#!/bin/sh
# FIXME: this is quick and dirty; replace with a more robust script snippet
for i in `sudo lsof -i | grep LISTEN | cut -d " " -f 1 |sort -u` ; do
  pack=`dpkg -S $i |grep bin |cut -f 1 -d : | uniq`
  echo "Service $i is installed by $pack";
  init=`dpkg -L $pack |grep init.d/ `
  if [ ! -z "$init" ]; then
    echo "and is run by $init"
  fi
done
```

						Once you find any unwanted services, remove the associated package (with dpkg --purge), or disable the service from starting automatically at boot time using update-rc.d (see [SectionÂ 3.5.1, âDisabling daemon servicesâ](ch03s05.en.md#disableserv)).
					
						For inetd services (launched by the superdaemon), check which services are enabled in /etc/inetd.conf using: 

```bash
$ grep -v "^#" /etc/inetd.conf | sort -u
```

						 Then disable those services that are not needed by commenting out the line that includes them in /etc/inetd.conf, removing the package, or using update-inetd.
					
						If you have wrapped services (those using /usr/sbin/tcpd), check that the files /etc/hosts.allow and /etc/hosts.deny are configured according to your service policy.
					
						If the server uses more than one external interface, depending on the service, you may want to limit the service to listen on a specific interface. For example, if you want internal FTP access only, make the FTP daemon listen only on your management interface, not on all interfaces (i.e, 0.0.0.0:21).
					
						Re-boot the machine, or switch to single user mode and then back to multiuser using the commands: 

```bash
# init 1
  (....)
  # init 2
```

						Check the services now available, and, if necessary, repeat the steps above.
					
						Now install the needed services, if you have not done so already, and configure them properly.
					
						Use the following shell command to determine what user each available service is running as: 

```bash
# for i in `/usr/sbin/lsof -i |grep LISTEN |cut -d " " -f 1 |sort -u`; \
  > do user=`ps ef |grep $i |grep -v grep |cut -f 1 -d " "` ; \
  > echo "Service $i is running as user $user"; done
```

						 Consider changing these services to a specific user/group and maybe chroot'ing them for increased security. You can do this by changing the /etc/init.d scripts which start the service. Most services in Debian use start-stop-daemon, which has options (--change-uid and --chroot) for accomplishing this. A word of warning regarding the chroot'ing of services: you may need to put all the files installed by the package (use dpkg -L) providing the service, as well as any packages it depends on, in the chroot'ed environment. Information about setting up a chroot environment for the ssh program can be found in [SectionÂ B.7, âChroot environment for SSHâ](chroot-ssh-env.en.md).
					
						Repeat the steps above in order to check that only desired services are running and that they are running as the desired user/group combination.
					
						Test the installed services in order to see if they work as expected.
					
						Check the system using a vulnerability assessment scanner (like nessus), in order to determine vulnerabilities in the system (i.e., misconfiguration, old services or unneeded services).
					
						Install network and host intrusion measures like snort and logcheck.
					
						Repeat the network scanner step and verify that the intrusion detection systems are working correctly.
					

			For the truly paranoid, also consider the following: 
			
						Add firewalling capabilities to the system, accepting incoming connections only to offered services and limiting outgoing connections only to those that are authorized.
					
						Re-check the installation with a new vulnerability assessment using a network scanner.
					
						Using a network scanner, check outbound connections from the system to an outside host and verify that unwanted connections do not find their way out.
					

			FIXME: this procedure considers service hardening but not system hardening at the user level, include information regarding checking user permissions, SETUID files and freezing changes in the system using the ext2 file system.
		[PrevAppendixÂ A.Â Changelog/History](changelog_history.en.md)[Up](#)[Home](index.en.md)[NextB.2.Â Configuration checklist](checklist.en.md)