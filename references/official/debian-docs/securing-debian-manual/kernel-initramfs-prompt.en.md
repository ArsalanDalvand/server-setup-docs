---
title: "4.5. Disable root prompt on the initramfs"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/kernel-initramfs-prompt.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/kernel-initramfs-prompt.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.5. Disable root prompt on the initramfs"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](lilo-passwd.en.md)Securing Debian Manual[Next](kernel-root-prompt.en.md)

## 4.5.Â Disable root prompt on the initramfs

			Note: This applies to the default kernels provided for releases after Debian 3.1
		
			Linux 2.6 kernels provide a way to access a root shell while booting which will be presented during loading the initramfs on error. This is helpful to permit the administrator to enter a rescue shell with root permissions. This shell can be used to manually load modules when autodetection fails. This behavior is the default for initramfs-tools generated initramfs. The following message will appear: 

```bash
"ALERT!  /dev/sda1 does not exist.  Dropping to a shell!
```

			In order to remove this behavior you need to set the following boot argument:panic=0. Add this to the variable GRUB_CMDLINE_LINUX in /etc/default/grub and issue update-grub or to the append section of /etc/lilo.conf.
		[Prev4.4.Â Set a LILO or GRUB password](lilo-passwd.en.md)[Up](#)[Home](index.en.md)[Next4.6.Â Remove root prompt on the kernel](kernel-root-prompt.en.md)