---
title: "4.7. Restricting console login access"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/restrict-console-login.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/restrict-console-login.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.7. Restricting console login access"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](kernel-root-prompt.en.md)Securing Debian Manual[Next](restrict-reboots.en.md)

## 4.7.Â Restricting console login access

			Some security policies might force administrators to log in to the system through the console with their user/password and then become superuser (with su or sudo). This policy is implemented in Debian by editing the /etc/pam.d/login and the /etc/securetty when using PAM:
		
/etc/pam.d/login In older Debian releases you would need to edit login.defs, and use the CONSOLE variable which defines a file or list of terminals on which root logins are allowed. enables the pam_securetty.so module. This module, when properly configured will not ask for a password when the root user tries to login on an insecure console, rejecting access as this user.
		
securetty The /etc/securetty is a configuration file that belongs to the login package. by adding/removing the terminals to which root access will be allowed. If you wish to allow only local console access then you need console, ttyX Or ttyvX in GNU/FreeBSD, and ttyE0 in GNU/KNetBSD. and vc/X (if using devfs devices), you might want to add also ttySX Or comX in GNU/Hurd, cuaaX in GNU/FreeBSD, and ttyXX in GNU/KNetBSD. if you are using a serial console for local access (where X is an integer, you might want to have multiple instances. The default configuration for Wheezy The default configuration in woody includes 12 local tty and vc consoles, as well as the console device but does not allow remote logins. In sarge the default configuration provides 64 consoles for tty and vc consoles. includes many tty devices, serial ports, vc consoles as well as the X server and the console device. You can safely adjust this if you are not using that many consoles. You can confirm the virtual consoles and the tty devices you have by reviewing /etc/inittab Look for the getty calls. . For more information on terminal devices read the [Text-Terminal-HOWTO](http://tldp.org/HOWTO/Text-Terminal-HOWTO-6.html)

			When using PAM, other changes to the login process, which might include restrictions to users and groups at given times, can be configured in /etc/pam.d/login. An interesting feature that can be disabled is the possibility to login with null (blank) passwords. This feature can be limited by removing nullok from the line: 

```bash
auth       required   pam_unix.so nullok
```

[Prev4.6.Â Remove root prompt on the kernel](kernel-root-prompt.en.md)[Up](#)[Home](index.en.md)[Next4.8.Â Restricting system reboots through the conso...](restrict-reboots.en.md)