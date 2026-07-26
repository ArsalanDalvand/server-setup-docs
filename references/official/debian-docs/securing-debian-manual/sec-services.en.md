---
title: "5.1.4. Restricing access to file transfer only"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/sec-services.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/sec-services.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "5.1.4. Restricing access to file transfer only"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch04s20.en.md)Securing Debian Manual[Next](ch05s02.en.md)

# ChapterÂ 5.Â Securing services running on your system

[5.1. Securing ssh](sec-services.en.md#idm2294)[5.1.1. Chrooting ssh](sec-services.en.md#ssh-chroot)[5.1.2. Ssh clients](sec-services.en.md#idm2365)[5.1.3. Disallowing file transfers](sec-services.en.md#idm2369)[5.1.4. Restricing access to file transfer only](sec-services.en.md#ssh-only-file)[5.2. Securing Squid](ch05s02.en.md)[5.3. Securing FTP](ftp-secure.en.md)[5.4. Securing access to the X Window System](ch05s04.en.md)[5.4.1. Check your display manager](ch05s04.en.md#idm2483)[5.5. Securing printing access (the lpd and lprng issue)](ch05s05.en.md)[5.6. Securing the mail service](ch05s06.en.md)[5.6.1. Configuring a Nullmailer](ch05s06.en.md#idm2531)[5.6.2. Providing secure access to mailboxes](ch05s06.en.md#idm2570)[5.6.3. Receiving mail securely](ch05s06.en.md#idm2595)[5.7. Securing BIND](sec-bind.en.md)[5.7.1. Bind configuration to avoid misuse](sec-bind.en.md#configure-bind)[5.7.2. Changing BIND's user](sec-bind.en.md#user-bind)[5.7.3. Chrooting the name server](sec-bind.en.md#chroot-bind)[5.8. Securing Apache](ch05s08.en.md)[5.8.1. Disabling users from publishing web contents](ch05s08.en.md#idm2764)[5.8.2. Logfiles permissions](ch05s08.en.md#idm2780)[5.8.3. Published web files](ch05s08.en.md#idm2783)[5.9. Securing finger](ch05s09.en.md)[5.10. General chroot and suid paranoia](chroot.en.md)[5.10.1. Making chrooted environments automatically](chroot.en.md#auto-chroot)[5.11. General cleartext password paranoia](ch05s11.en.md)[5.12. Disabling NIS](ch05s12.en.md)[5.13. Securing RPC services](rpc.en.md)[5.13.1. Disabling RPC services completely](rpc.en.md#idm2871)[5.13.2. Limiting access to RPC services](rpc.en.md#idm2878)[5.14. Adding firewall capabilities](firewall-setup.en.md)[5.14.1. Firewalling the local system](firewall-setup.en.md#idm2909)[5.14.2. Using a firewall to protect other systems](firewall-setup.en.md#idm2933)[5.14.3. Setting up a firewall](firewall-setup.en.md#idm2942)
		Services can be secured in a running system in two ways: 
		
					Making them only accessible at the access points (interfaces) they need to be in.
				
					Configuring them properly so that they can only be used by legitimate users in an authorized manner.
				

		Restricting services so that they can only be accessed from a given place can be done by restricting access to them at the kernel (i.e. firewall) level, configure them to listen only on a given interface (some services might not provide this feature) or using some other methods, for example the Linux vserver patch (for 2.4.16) can be used to force processes to use only one interface.
	
		Regarding the services running from inetd (telnet, ftp, finger, pop3...) it is worth noting that inetd can be configured so that services only listen on a given interface (using service@ip syntax) but that's an undocumented feature. One of its substitutes, the xinetd meta-daemon includes a bind option just for this matter. See ixnetd.conf(5) manual page. 

```bash
service nntp
{
        socket_type     = stream
        protocol        = tcp
        wait            = no
        user            = news
        group           = news
        server          = /usr/bin/env
        server_args     = POSTING_OK=1 PATH=/usr/sbin/:/usr/bin:/sbin/:/bin
+/usr/sbin/snntpd logger -p news.info
        bind            = 127.0.0.1
}
```

		The following sections detail how specific individual services can be configured properly depending on their intended use.
	

## 5.1.Â Securing ssh

			If you are still running telnet instead of ssh, you should take a break from this manual and change this. Ssh should be used for all remote logins instead of telnet. In an age where it is easy to sniff Internet traffic and get clear-text passwords, you should use only protocols which use cryptography. So, perform an apt-get install ssh on your system now.
		
			Encourage all the users on your system to use ssh instead of telnet, or even better, uninstall telnet/telnetd. In addition you should avoid logging into the system using ssh as root and use alternative methods to become root instead, like su or sudo. Finally, the sshd_config file, in /etc/ssh, should be modified to increase security as well: 
			
ListenAddress 192.168.0.1 Have ssh listen only on a given interface, just in case you have more than one (and do not want ssh available on it) or in the future add a new network card (and don't want ssh connections from it).
					
PermitRootLogin no Try not to permit Root Login wherever possible. If anyone wants to become root via ssh, now two logins are needed and the root password cannot be brute forced via SSH.
					
Port 666 or ListenAddress 192.168.0.1:666 Change the listen port, so the intruder cannot be completely sure whether a sshd daemon runs (be forewarned, this is security by obscurity).
					
PermitEmptyPasswords no Empty passwords make a mockery of system security.
					
AllowUsers alex ref me@somewhere Allow only certain users to have access via ssh to this machine. user@host can also be used to restrict a given user from accessing only at a given host.
					
AllowGroups wheel admin Allow only certain group members to have access via ssh to this machine. AllowGroups and AllowUsers have equivalent directives for denying access to a machine. Not surprisingly they are called "DenyUsers" and "DenyGroups".
					
PasswordAuthentication yes It is completely your choice what you want to do. It is more secure to only allow access to the machine from users with ssh-keys placed in the ~/.ssh/authorized_keys file. If you want so, set this one to "no".
					
						Disable any form of authentication you do not really need, if you do not use, for example RhostsRSAAuthentication, HostbasedAuthentication, KerberosAuthentication or RhostsAuthentication you should disable them, even if they are already by default (see the manpage sshd_config(5) manual page).
					
Protocol 2 Disable the protocol version 1, since it has some design flaws that make it easier to crack passwords. For more information read [http://earthops.net/ssh-timing.pdf](http://earthops.net/ssh-timing.pdf) or the [http://xforce.iss.net/static/6449.php](http://xforce.iss.net/static/6449.php).
					
Banner /etc/some_file Add a banner (it will be retrieved from the file) to users connecting to the ssh server. In some countries sending a warning before access to a given system about unauthorized access or user monitoring should be added to have legal protection.
					

			You can also restrict access to the ssh server using pam_listfile or pam_wheel in the PAM control file. For example, you could keep anyone not listed in /etc/loginusers away by adding this line to /etc/pam.d/ssh: 

```bash
auth       required     pam_listfile.so sense=allow onerr=fail item=user file=/etc/loginusers
```

			As a final note, be aware that these directives are from a OpenSSH configuration file. Right now, there are three commonly used SSH daemons, ssh1, ssh2, and OpenSSH by the OpenBSD people. Ssh1 was the first ssh daemon available and it is still the most commonly used (there are rumors that there is even a Windows port). Ssh2 has many advantages over ssh1 except it is released under a closed-source license. OpenSSH is completely free ssh daemon, which supports both ssh1 and ssh2. OpenSSH is the version installed on Debian when the package ssh is chosen.
		
			You can read more information on how to set up SSH with PAM support in the [http://lists.debian.org/debian-security/2001/11/msg00395.html](http://lists.debian.org/debian-security/2001/11/msg00395.html).
		

### 5.1.1.Â Chrooting ssh

				Currently OpenSSH does not provide a way to chroot automatically users upon connection (the commercial version does provide this functionality). However there is a project to provide this functionality for OpenSSH too, see [http://chrootssh.sourceforge.net](http://chrootssh.sourceforge.net), it is not currently packaged for Debian, though. You could use, however, the pam_chroot module as described in [SectionÂ 4.11.15, âRestricting users's accessâ](ch04s11.en.md#user-restrict).
			
				In [SectionÂ B.7, âChroot environment for SSHâ](chroot-ssh-env.en.md) you can find several options to make a chroot environment for SSH.
			

### 5.1.2.Â Ssh clients

				If you are using an SSH client against the SSH server you must make sure that it supports the same protocols that are enforced on the server. For example, if you use the mindterm package, it only supports protocol version 1. However, the sshd server is, by default, configured to only accept version 2 (for security reasons).
			

### 5.1.3.Â Disallowing file transfers

				If you do not want users to transfer files to and from the ssh server you need to restrict access to the sftp-serverand the scp access. You can restrict sftp-server by configuring the proper Subsystem in the /etc/ssh/sshd_config.
			
				You can also chroot users (using libpam-chroot so that, even if file transfer is allowed, they are limited to an environment which does not include any system files.
			

### 5.1.4.Â Restricing access to file transfer only

				You might want to restrict access to users so that they can only do file transfers and cannot have interactive shells. In order to do this you can either: 
				
							disallow users from login to the ssh server (as described above either through the configuration file or PAM configuration).
						
							give users a restricted shell such as scponly or rssh. These shells restrict the commands available to the users so that they are not provided any remote execution priviledges.
						
[Prev4.20.Â Other recommendations](ch04s20.en.md)[Up](#)[Home](index.en.md)[Next5.2.Â Securing Squid](ch05s02.en.md)