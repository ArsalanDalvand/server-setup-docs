---
title: "4.9. Restricting the use of the Magic SysRq key"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/restrict-sysrq.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/restrict-sysrq.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.9. Restricting the use of the Magic SysRq key"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](restrict-reboots.en.md)Securing Debian Manual[Next](ch04s10.en.md)

## 4.9.Â Restricting the use of the Magic SysRq key

			The Magic SysRq key is a key combination that allows users connected to the system console of a Linux kernel to perform some low-level commands. These low-level commands are sent by pressing simultaneously Alt+SysRq and a command key. The SysRq key in many keyboards is labeled as the Print Screen key.
		
			Since the Etch release, the Magic SysRq key feature is enabled in the Linux kernel to allow console users certain privileges. You can confirm this by checking if the /proc/sys/kernel/sysrq exists and reviewing its value: 

```bash
$ cat /proc/sys/kernel/sysrq 
438
```

			The default value shown above allows all of the SysRq functions except for the possibility of sending signals to processes. For example, it allow users connected to the console to remount all systems read-only, reboot the system or cause a kernel panic. In all the features are enabled, or in older kernels (earlier than 2.6.12) the value will be just 1.
		
			You should disable this functionality ifaccess to the console is not restricted to authorised users: the console is connected to a modem line, there is easy physical access to the system or it is running in a virtualised environment and other users access the console. To do this edit the /etc/sysctl.conf and add the following lines: 

```bash
# Disables the magic SysRq key
kernel.sysrq = 0
```

			For more information, read [security chapter in the Remote Serial Console HOWTO](http://tldp.org/HOWTO/Remote-Serial-Console-HOWTO/security-sysrq.html), [Kernel SysRQ documentation](https://www.kernel.org/doc/Documentation/admin-guide/sysrq.rst). and the [Magic_SysRq_key wikipedia entry](http://en.wikipedia.org/wiki/Magic_SysRq_key).
		[Prev4.8.Â Restricting system reboots through the conso...](restrict-reboots.en.md)[Up](#)[Home](index.en.md)[Next4.10.Â Mounting partitions the right way](ch04s10.en.md)