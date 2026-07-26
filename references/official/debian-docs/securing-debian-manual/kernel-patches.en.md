---
title: "4.14. Adding kernel patches"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/kernel-patches.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/kernel-patches.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.14. Adding kernel patches"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](log-alerts.en.md)Securing Debian Manual[Next](ch04s15.en.md)

## 4.14.Â Adding kernel patches

			Debian GNU/Linux provides some of the patches for the Linux kernel that enhance its security. These include: 
			
[Linux Intrusion Detection](http://www.lids.org) provided in the kernel-patch-2.4-lids package. This kernel patch makes the process of hardening your Linux system easier by allowing you to restrict, hide and protect processes, even from root. It implements mandatory access control capabilities.
					
[Linux Trustees](http://trustees.sourceforge.net/), provided in package trustees. This patch adds a decent advanced permissions management system to your Linux kernel. Special objects (called trustees) are bound to every file or directory, and are stored in kernel memory, which allows fast lookup of all permissions.
					
						NSA Enhanced Linux (in package selinux). Backports of the SElinux-enabled packages are available at [https://salsa.debian.org/selinux-team](https://salsa.debian.org/selinux-team). More information available at [SElinux in Debian Wiki page](http://wiki.debian.org/SELinux), at [Manoj Srivastava's](http://www.golden-gryphon.com/software/security/selinux.xhtml) and [Russell Cookers's](http://www.coker.com.au/selinux/) SElinux websites.
					
						The kernel patch [http://people.redhat.com/mingo/exec-shield](http://people.redhat.com/mingo/exec-shield) provided in the kernel-patch-exec-shield package. This patch provides protection against some buffer overflows (stack smashing attacks).
					
						The [Grsecurity patch](http://www.grsecurity.net/), provided by the kernel-patch-2.4-grsecurity and kernel-patch-grsecurity2 packages [[28]](#ftn.idm1860) implements Mandatory Access Control through RBAC, provides buffer overflow protection through PaX, ACLs, network randomness (to make OS fingerprinting more difficult) and [many more features](http://www.grsecurity.net/features.php).
					
						The kernel-patch-adamantix provides the patches developed for [Adamantix](http://www.adamantix.org/), a Debian-based distribution. This kernel patch for the 2.4.x kernel releases introduces some security features such as a non-executable stack through the use of [http://pageexec.virtualave.net/](http://pageexec.virtualave.net/) and mandatory access control based on [http://www.rsbac.org/](http://www.rsbac.org/). Other features include: [http://www.vanheusden.com/Linux/sp/](http://www.vanheusden.com/Linux/sp/), AES encrypted loop device, MPPE support and an IPSEC v2.6 backport.
					
cryptoloop-source. This patches allows you to use the functions of the kernel crypto API to create encrypted filesystems using the loopback device.
					
						IPSEC kernel support (in package linux-patch-openswan). If you want to use the IPsec protocol with Linux, you need this patch. You can create VPNs with this quite easily, even to Windows machines, as IPsec is a common standard. IPsec capabilities have been added to the 2.5 development kernel, so this feature will be present by default in the future Linux Kernel 2.6. Homepage: [http://www.openswan.org](http://www.openswan.org). FIXME: The latest 2.4 kernels provided in Debian include a backport of the IPSEC code from 2.5. Comment on this.
					

			The following security kernel patches are only available for old kernel versions in woody and are deprecated: 
			
[http://acl.bestbits.at/](http://acl.bestbits.at/) (ACLs) for Linux provided in the package kernel-patch-acl. This kernel patch adds access control lists, an advanced method for restricting access to files. It allows you to control fine-grain access to files and directory.
					
						The [http://www.openwall.com/linux/](http://www.openwall.com/linux/) linux kernel patch by Solar Designer, provided in the kernel-patch-2.2.18-openwall package. This is a useful set of kernel restrictions, like restricted links, FIFOs in /tmp, a restricted /proc file system, special file descriptor handling, non-executable user stack area and other features. Note: This package applies to the 2.2 release, no packages are available for the 2.4 release patches provided by Solar.
					
kernel-patch-int. This patch also adds cryptographic capabilities to the Linux kernel, and was useful with Debian releases up to Potato. It doesn't work with Woody, and if you are using Sarge or a newer version, you should use a more recent kernel which includes these features already.
					

			However, some patches have not been provided in Debian yet. If you feel that some of these should be included please ask for it at the [http://www.debian.org/devel/wnpp/](http://www.debian.org/devel/wnpp/).
		[[28]](#idm1860)
							Notice that this patch conflicts with patches already included in Debian's 2.4 kernel source package. You will need to use the stock vanilla kernel. You can do this with the following steps: 

```bash
# apt-get install kernel-source-2.4.22 kernel-patch-debian-2.4.22
# tar xjf /usr/src/kernel-source-2.4.22.tar.bz2
# cd kernel-source-2.4.22
# /usr/src/kernel-patches/all/2.4.22/unpatch/debian
```

							 For more information see [http://bugs.debian.org/194225](http://bugs.debian.org/194225), [http://bugs.debian.org/199519](http://bugs.debian.org/199519), [http://bugs.debian.org/206458](http://bugs.debian.org/206458), [http://bugs.debian.org/203759](http://bugs.debian.org/203759), [http://bugs.debian.org/204424](http://bugs.debian.org/204424), [http://bugs.debian.org/210762](http://bugs.debian.org/210762), [http://bugs.debian.org/211213](http://bugs.debian.org/211213), and the [http://lists.debian.org/debian-devel/2003/09/msg01133.html](http://lists.debian.org/debian-devel/2003/09/msg01133.html)
[Prev4.13.Â The importance of logs and alerts](log-alerts.en.md)[Up](#)[Home](index.en.md)[Next4.15.Â Protecting against buffer overflows](ch04s15.en.md)