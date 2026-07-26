---
title: "5.3. Securing FTP"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/ftp-secure.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/ftp-secure.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "5.3. Securing FTP"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch05s02.en.md)Securing Debian Manual[Next](ch05s04.en.md)

## 5.3.Â Securing FTP

			If you really have to use FTP (without wrapping it with sslwrap or inside a SSL or SSH tunnel), you should chroot ftp into the ftp users' home directory, so that the user is unable to see anything else than their own directory. Otherwise they could traverse your root file system just like if they had a shell in it. You can add the following line in your proftpd.conf in your global section to enable this chroot feature: 

```bash
DefaultRoot ~
```

			Restart ProFTPd by /etc/init.d/proftpd restart and check whether you can escape from your homedir now.
		
			To prevent ProFTPd DoS attacks using ../../.., add the following line in /etc/proftpd.conf: DenyFilter \*.*/

			Always remember that FTP sends login and authentication passwords in clear text (this is not an issue if you are providing an anonymous public service) and there are better alternatives in Debian for this. For example, sftp (provided by ssh). There are also free implementations of SSH for other operating systems: [http://www.chiark.greenend.org.uk/~sgtatham/putty/](http://www.chiark.greenend.org.uk/~sgtatham/putty/) and [http://www.cygwin.com](http://www.cygwin.com) for example. 
		
			However, if you still maintain the FTP server while making users access through SSH you might encounter a typical problem. Users accessing anonymous FTP servers inside SSH-secured systems might try to log in the FTP server. While the access will be refused, the password will nevertheless be sent through the net in clear form. To avoid that, ProFTPd developer TJ Saunders has created a patch that prevents users feeding the anonymous FTP server with valid SSH accounts. More information and patch available at: [http://www.castaglia.org/proftpd/#Patches](http://www.castaglia.org/proftpd/#Patches). This patch has been reported to Debian too, see [http://bugs.debian.org/145669](http://bugs.debian.org/145669).
		[Prev5.2.Â Securing Squid](ch05s02.en.md)[Up](#)[Home](index.en.md)[Next5.4.Â Securing access to the X Window System](ch05s04.en.md)