---
title: "11.5.2. Samba Client"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.windows-file-server-with-samba.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.windows-file-server-with-samba.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "11.5.2. Samba Client"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.nfs-file-server.en.md)The Debian Administrator's Handbook[Next](sect.http-ftp-proxy.en.md)

## 11.5.Â Setting Up Windows Shares with Samba

			Samba is a suite of tools handling the SMB protocol (also known as âCIFSâ) on Linux. This protocol is used by Windows for network shares and shared printers.
		
			Samba can also act as a Windows domain controller. This is an outstanding tool for ensuring seamless integration of Linux servers and the office desktop machines still running Windows.
		

### 11.5.1.Â Samba Server

				The samba package contains the main two servers of SambaÂ 4, smbd and nmbd.
			

DOCUMENTATION Going further

				The Samba server is extremely configurable and versatile, and can address a great many different use cases matching very different requirements and network architectures. This book only focuses on the use case where Samba is used as a standalone server, but it can also be an NT4 Domain Controller or a full Active Directory Domain Controller, or a simple member of an existing domain (which could be managed by a Windows server).
			
				The samba package contains all the necessary manual pages and in /usr/share/doc/samba/examples/ a wealth of commented example files. If you are looking for a more comprehensive documentation, you may check the Samba website. âÂ [https://www.samba.org/samba/docs/](https://www.samba.org/samba/docs/)

TOOL Authenticating with a Windows Server

				Winbind gives system administrators the option of using a Windows server as an authentication server. Winbind also integrates cleanly with PAM and NSS. This allows setting up Linux machines where all users of a Windows domain automatically get an account.
			
				More information can be found in the /usr/share/doc/libpam-winbind/examples/pam_winbind/ directory of the libpam-winbind package.
			

#### 11.5.1.1.Â Configuring with debconf

					The package sets up a minimal configuration during the initial installation in /etc/samba/smb.conf by plainly copying /usr/share/samba/smb.conf. So you should really run dpkg-reconfigure samba-common to adapt it:
				
					On first installation the only piece of required information is the name of the workgroup where the Samba server will belong (the answer is FALCOTNET in our case).
				
					In case of a package update (from the old stable Debian version) or if the SMB server has already been configured to use a WINS server (wins server), the package also proposes identifying the WINS server from the information provided by the DHCP daemon. The Falcot Corp administrators rejected this option, since they intend to use the Samba server itself as the WINS server.
				

#### 11.5.1.2.Â Configuring Manually

##### 11.5.1.2.1.Â Changes to smb.conf

						The requirements at Falcot require other options to be modified in the /etc/samba/smb.conf configuration file. The following excerpts summarize the changes that were effected in the [global] section.
					

```bash
[...]

[global]

## Browsing/Identification ###

# Change this to the workgroup/NT-domain name your Samba server will part of
   workgroup = FALCOTNET

[...]

# Windows Internet Name Serving Support Section:
# WINS Support - Tells the NMBD component of Samba to enable its WINS Server
   wins support = yes 

[...]

####### Authentication #######

# Server role. Defines in which mode Samba will operate. Possible
# values are "standalone server", "member server", "classic primary
# domain controller", "classic backup domain controller", "active
# directory domain controller".
#
# Most people will want "standalone server" or "member server".
# Running as "active directory domain controller" will require first
# running "samba-tool domain provision" to wipe databases and create a
# new domain.
   server role = standalone server

   obey pam restrictions = yes

[...]

# "security = user" is always a good idea. This will require a Unix account
# in this server for every user accessing the server.
   security = user 

[...]
```

[](#smb.conf.wins)

								Indicates that Samba should act as a Netbios name server (WINS) for the local network. This option had been removed from the default configuration in Buster and must be added manually if desired.
							

[](#smb.conf.security)

								This is the default value for this parameter; however, since it is central to the Samba configuration, filling it explicitly is recommended. Each user must authenticate before accessing any share.
							

##### 11.5.1.2.2.Â Adding Users

						Each Samba user needs an account on the server; the Unix accounts must be created first, then the user needs to be registered in Samba's database. The Unix step is done quite normally (using adduser for instance).
					
						Adding an existing user to the Samba database is a matter of running the smbpasswd -a user command; this command asks for the password interactively.
					
						A user can be deleted with the smbpasswd -x user command. A Samba account can also be temporarily disabled (with smbpasswd -d user) and re-enabled later (with smbpasswd -e user).
					

### 11.5.2.Â Samba Client

				The client features in Samba allow a Linux machine to access Windows shares and shared printers. The required programs are available in the cifs-utils and smbclient packages.
			

#### 11.5.2.1.Â The smbclient Program

					The smbclient program queries SMB servers. It accepts a -U user option, for connecting to the server under a specific identity. smbclient //server/share accesses the share in an interactive way similar to the command-line FTP client. smbclient -L server lists all available (and visible) shares on a server.
				

#### 11.5.2.2.Â Mounting Windows Shares

					The mount command allows mounting a Windows share into the Linux filesystem hierarchy (with the help of mount.cifs provided by cifs-utils).
				

ExampleÂ 11.21.Â Mounting a Windows share

```bash
mount -t cifs //arrakis/shared /shared \
      -o credentials=/etc/smb-credentials
```

					The /etc/smb-credentials file (which must not be readable by users) has the following format:
				

```bash
username = user
password = password
```

					Other options can be specified on the command-line; their full list is available in the mount.cifs(1) manual page. Two options in particular can be interesting: uid and gid allow forcing the owner and group of files available on the mount, so as not to restrict access to root.
				
					A mount of a Windows share can also be configured in /etc/fstab:
				

```bash
//server/shared /shared cifs credentials=/etc/smb-credentials
```

					Unmounting an SMB/CIFS share is done with the standard umount command.
				

#### 11.5.2.3.Â Printing on a Shared Printer

					CUPS is an elegant solution for printing from a Linux workstation to a printer shared by a Windows machine. When the smbclient is installed, CUPS allows installing Windows shared printers automatically.
				
					Here are the required steps:
				
							Enter the CUPS configuration interface: http://localhost:631/admin

							Click on âAdd Printerâ.
						
							Choose the printer device, pick âWindows Printer via SAMBAâ.
						
							Enter the connection URI for the network printer. It should look like the following:
						
smb://user:password@server/printer.
						
							Enter the name that will uniquely identify this printer. Then enter the description and location of the printer. Those are the strings that will be shown to end users to help them identify the printers.
						
							Indicate the manufacturer/model of the printer, or directly provide a working printer description file (PPD).
						
					VoilÃ , the printer is operational!
				[Prev11.4.Â NFS File Server](sect.nfs-file-server.en.md)[Up](#)[Home](index.en.md)[Next11.6.Â HTTP/FTP Proxy](sect.http-ftp-proxy.en.md)