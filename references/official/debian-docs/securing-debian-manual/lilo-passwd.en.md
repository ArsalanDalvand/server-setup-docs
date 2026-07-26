---
title: "4.4. Set a LILO or GRUB password"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/lilo-passwd.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/lilo-passwd.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.4. Set a LILO or GRUB password"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](bios-boot.en.md)Securing Debian Manual[Next](kernel-initramfs-prompt.en.md)

## 4.4.Â Set a LILO or GRUB password

			Anybody can easily get a root-shell and change your passwords by entering 

```bash
<name-of-your-bootimage> init=/bin/sh
```

			 at the boot prompt. After changing the passwords and rebooting the system, the person has unlimited root-access and can do anything he/she wants to the system. After this procedure you will not have root access to your system, as you do not know the root password.
		
			To make sure that this cannot happen, you should set a password for the boot loader. You can choose between a global password or a password for a certain image.
		
			For LILO you need to edit the config file /etc/lilo.conf and add a password and restricted line as in the example below. 

```bash
image=/boot/2.2.14-vmlinuz
     label=Linux
     read-only
     password=hackme
     restricted
```

			Then, make sure that the configuration file is not world readable to prevent local users from reading the password. When done, rerun lilo. Omitting the restricted line causes lilo to always prompt for a password, regardless of whether LILO was passed parameters. The default permissions for /etc/lilo.conf grant read and write permissions to root, and enable read-only access for lilo.conf's group, root.
		
			If you use GRUB instead of LILO, edit /boot/grub/menu.lst and add the following two lines at the top (substituting, of course hackme with the desired password). This prevents users from editing the boot items. timeout 3 specifies a 3 second delay before grub boots the default item. 

```bash
timeout 3
  password hackme
```

			To further harden the integrity of the password, you may store the password in an encrypted form. The utility grub-md5-crypt generates a hashed password which is compatible with GRUB's encrypted password algorithm (MD5). To specify in grub that an MD5 format password will be used, use the following directive: 

```bash
timeout 3
  password --md5 $1$bw0ez$tljnxxKLfMzmnDVaQWgjP0
```

			 The --md5 parameter was added to instruct grub to perform the MD5 authentication process. The provided password is the MD5 encrypted version of hackme. Using the MD5 password method is preferable to choosing its clear-text counterpart. More information about grub passwords may be found in the grub-doc package.
		[Prev4.3.Â Change the BIOS (again)](bios-boot.en.md)[Up](#)[Home](index.en.md)[Next4.5.Â Disable root prompt on the initramfs](kernel-initramfs-prompt.en.md)