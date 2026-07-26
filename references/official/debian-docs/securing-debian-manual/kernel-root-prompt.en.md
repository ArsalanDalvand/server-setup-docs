---
title: "4.6. Remove root prompt on the kernel"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/kernel-root-prompt.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/kernel-root-prompt.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.6. Remove root prompt on the kernel"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](kernel-initramfs-prompt.en.md)Securing Debian Manual[Next](restrict-console-login.en.md)

## 4.6.Â Remove root prompt on the kernel

			Note: This does not apply to the kernels provided for Debian 3.1 as the timeout for the kernel delay has been changed to 0.
		
			Linux 2.4 kernels provide a way to access a root shell while booting which will be presented just after loading the cramfs file system. A message will appear to permit the administrator to enter an executable shell with root permissions, this shell can be used to manually load modules when autodetection fails. This behavior is the default for initrd's linuxrc. The following message will appear: 

```bash
Press ENTER to obtain a shell (waits 5 seconds)
```

			In order to remove this behavior you need to change /etc/mkinitrd/mkinitrd.conf and set: 

```bash
# DELAY  The  number  of seconds the linuxrc script should wait to
  # allow the user to interrupt it before the system is brought up
  DELAY=0
```

			Then regenerate your ramdisk image. You can do this for example with: 

```bash
# cd /boot
  # mkinitrd -o initrd.img-2.4.18-k7 /lib/modules/2.4.18-k7
```

			or (preferred): 

```bash
# dpkg-reconfigure -plow kernel-image-2.4.x-yz
```

[Prev4.5.Â Disable root prompt on the initramfs](kernel-initramfs-prompt.en.md)[Up](#)[Home](index.en.md)[Next4.7.Â Restricting console login access](restrict-console-login.en.md)