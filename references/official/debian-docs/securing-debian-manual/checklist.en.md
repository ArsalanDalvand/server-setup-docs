---
title: "B.2. Configuration checklist"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/checklist.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/checklist.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "B.2. Configuration checklist"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](apb.en.md)Securing Debian Manual[Next](snort-box.en.md)

## B.2.Â Configuration checklist

			This appendix briefly reiterates points from other sections in this manual in a condensed checklist format. This is intended as a quick summary for someone who has already read the manual. There are other good checklists available, including Kurt Seifried's [http://seifried.org/security/os/linux/20020324-securing-linux-step-by-step.html](http://seifried.org/security/os/linux/20020324-securing-linux-step-by-step.html) and [http://www.cert.org/tech_tips/usc20_full.html](http://www.cert.org/tech_tips/usc20_full.html). 
		
			FIXME: This is based on v1.4 of the manual and might need to be updated.
		
					Limit physical access and booting capabilities 
					
								Enable a password in the BIOS.
							
								Disable floppy/cdrom/... booting in the system's BIOS.
							
								Set a LILO or GRUB password (/etc/lilo.conf or /boot/grub/menu.lst, respectively); check that the LILO or GRUB configuration file is read-protected.
							

					Partitioning 
					
								Separate user-writable data, non-system data, and rapidly changing run-time data to their own partitions
							
								Set nosuid,noexec,nodev mount options in /etc/fstab on ext2/3 partitions that should not hold binaries such as /home or /tmp.
							

					Password hygiene and login security 
					
								Set a good root password
							
								Install and use PAM 
								
											Add MD5 support to PAM and make sure that (generally speaking) entries in /etc/pam.d/ files which grant access to the machine have the second field in the pam.d file set to requisite or required.
										
											Tweak /etc/pam.d/login so as to only permit local root logins.
										
											Also mark authorized tty:s in /etc/security/access.conf and generally set up this file to limit root logins as much as possible.
										
											Add pam_limits.so if you want to set per-user limits
										
											Tweak /etc/pam.d/passwd: set minimum length of passwords higher (6 characters maybe) and enable MD5
										
											Add group wheel to /etc/group if desired; add pam_wheel.so group=wheel entry to /etc/pam.d/su

											For custom per-user controls, use pam_listfile.so entries where appropriate
										
											Have an /etc/pam.d/other file and set it up with tight security
										

								Set up limits in /etc/security/limits.conf (note that /etc/limits is not used if you are using PAM)
							
								Tighten up /etc/login.defs; also, if you enabled MD5 and/or PAM, make sure you make the corresponding changes here, too
							
								Tighten up /etc/pam.d/login

								Disable root ftp access in /etc/ftpusers

								Disable network root login; use su(1) or sudo(1). (consider installing sudo)
							
								Use PAM to enforce additional constraints on logins?
							

					Other local security issues 
					
								Kernel tweaks (see [SectionÂ 4.18.1, âConfiguring kernel network featuresâ](network-secure.en.md#kernel-conf))
							
								Kernel patches (see [SectionÂ 4.14, âAdding kernel patchesâ](kernel-patches.en.md))
							
								Tighten up log file permissions (/var/log/{last,fail}log, Apache logs)
							
								Verify that SETUID checking is enabled in /etc/checksecurity.conf

								Consider making some log files append-only and configuration files immutable using chattr (ext2/3 file systems only)
							
								Set up file integrity (see [SectionÂ 4.17.3, âChecking file system integrityâ](ch04s17.en.md#check-integ)). Install debsums

								Log everything to a local printer?
							
								Burn your configuration on a boot-able CD and boot off that?
							
								Disable kernel modules?
							

					Limit network access 
					
								Install and configure ssh (suggest PermitRootLogin No in /etc/ssh/sshd_config, PermitEmptyPasswords No; note other suggestions in text also)
							
								Disable or remove in.telnetd, if installed
							
								Generally, disable gratuitous services in /etc/inetd.conf using update-inetd --disable (or disable inetd altogether, or use a replacement such as xinetd or rlinetd)
							
								Disable other gratuitous network services; ftp, DNS, WWW etc should not be running if you do not need them and monitor them regularly. In most cases mail should be running but configured for local delivery only.
							
								For those services which you do need, do not just use the most common programs, look for more secure versions shipped with Debian (or from other sources). Whatever you end up running, make sure you understand the risks.
							
								Set up chroot jails for outside users and daemons.
							
								Configure firewall and tcpwrappers (i.e. hosts_access(5)); note trick for /etc/hosts.deny in text.
							
								If you run ftp, set up your ftpd server to always run chroot'ed to the user's home directory
							
								If you run X, disable xhost authentication and go with ssh instead; better yet, disable remote X if you can (add -nolisten tcp to the X command line and turn off XDMCP in /etc/X11/xdm/xdm-config by setting the requestPort to 0)
							
								Disable remote access to printers
							
								Tunnel any IMAP or POP sessions through SSL or ssh; install stunnel if you want to provide this service to remote mail users
							
								Set up a log host and configure other machines to send logs to this host (/etc/syslog.conf)
							
								Secure BIND, Sendmail, and other complex daemons (run in a chroot jail; run as a non-root pseudo-user)
							
								Install tiger or a similar network intrusion detection tool.
							
								Install snort or a similar network intrusion detection tool.v
							
								Do without NIS and RPC if you can (disable portmap).
							

					Policy issues 
					
								Educate users about the whys and hows of your policies. When you have prohibited something which is regularly available on other systems, provide documentation which explains how to accomplish similar results using other, more secure means.
							
								Prohibit use of protocols which use clear-text passwords (telnet, rsh and friends; ftp, imap, http, ...).
							
								Prohibit programs which use SVGAlib.
							
								Use disk quotas.
							

					Keep informed about security issues 
					
								Subscribe to security mailing lists
							
								Configure apt for security updates -- add to /etc/apt/sources.list an entry (or entries) for http://security.debian.org/
							
								Also remember to periodically run apt-get update ; apt-get upgrade (perhaps install as a cron job?) as explained in [SectionÂ 4.2, âExecute a security updateâ](security-update.en.md).
							
[PrevAppendixÂ B.Â Appendix](apb.en.md)[Up](#)[Home](index.en.md)[NextB.3.Â Setting up a stand-alone IDS](snort-box.en.md)