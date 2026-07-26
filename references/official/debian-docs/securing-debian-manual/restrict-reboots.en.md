---
title: "4.8. Restricting system reboots through the console"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/restrict-reboots.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/restrict-reboots.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.8. Restricting system reboots through the console"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](restrict-console-login.en.md)Securing Debian Manual[Next](restrict-sysrq.en.md)

## 4.8.Â Restricting system reboots through the console

			If your system has a keyboard attached to it anyone (yes anyone) with physical access to the system can reboot the system through it without login in just pressing the Ctrl+Alt+Delete keyboard combination, also known as the three finger salute. This might, or might not, adhere to your security policy.
		
			This is aggravated in environments in which the operating system is running virtualised. In these environments, the possibility extends to users that have access to the virtual console (which might be accessed over the network). Also note that, in these environments, this keyboard combination is used constantly (to open a login shell in some GUI operating systems) and an administrator might virtually send it and force a system reboot.
		
			There are two ways to restrict this: 
			
						configure it so that only allowed users can reboot the system,
					
						disable this feature completely.
					

			If you want to restrict this, you must check the /etc/inittab so that the line that includes ctrlaltdel calls shutdown with the -a switch.
		
			The default in Debian includes this switch: 

```bash
ca:12345:ctrlaltdel:/sbin/shutdown -t1 -a -r now
```

			The -a switch, as the shutdown(8) manpage describes,makes it possible to allow some users to shutdown the system. For this the file /etc/shutdown.allow must be created and the administrator has to include there the name of users which can boot the system. When the three finger salute combination is pressed in a console the program will check if any of the users listed in the file are logged in. If none of them is, shutdown will not reboot the system.
		
			If you want to disable the Ctrl+Alt+Del combination you just need to comment the line with the ctrlaltdel definition in the /etc/inittab.
		
			Remember to run init q after making any changes to the /etc/inittab file for the changes to take effect.
		[Prev4.7.Â Restricting console login access](restrict-console-login.en.md)[Up](#)[Home](index.en.md)[Next4.9.Â Restricting the use of the Magic SysRq key](restrict-sysrq.en.md)