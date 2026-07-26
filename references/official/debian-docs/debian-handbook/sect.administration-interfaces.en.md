---
title: "9.4.2. Configuring Packages:debconf"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.administration-interfaces.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.administration-interfaces.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.4.2. Configuring Packages:debconf"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.rights-management.en.md)The Debian Administrator's Handbook[Next](sect.syslog.en.md)

## 9.4.Â Administration Interfaces

			Using a graphical interface for administration is interesting in various circumstances. An administrator does not necessarily know all the configuration details for all their services, and doesn't always have the time to go seeking out the documentation on the matter. A graphical interface for administration can thus accelerate the deployment of a new service. It can also simplify the setup of services which are hard to configure.
		
			Such an interface is only an aid, and not an end in itself. In all cases, the administrator must master its behavior in order to understand and work around any potential problem.
		
			Since no interface is perfect, you may be tempted to try several solutions. This is to be avoided as much as possible, since different tools are sometimes incompatible in their work methods. Even if they all aim to be very flexible and try to adopt the configuration file as a single reference, they are not always able to integrate external changes.
		

### 9.4.1.Â Administrating on a Web Interface: webmin

				This is, without a doubt, one of the most successful administration interfaces. It is a modular system managed through a web browser, covering a wide array of areas and tools. Furthermore, it is internationalized and available in many languages.
			

FigureÂ 9.5.Â Webmin dashboard

				Sadly, webmin is no longer part of Debian. Its Debian maintainer removed the packages created because they no longer had the time required to maintain them at an acceptable quality level. Nobody has officially taken over, so Debian does not provide the webmin package.
			
				There is, however, an unofficial package distributed on the webmin.com website. Contrary to the original Debian packages, this package is monolithic; all of its configuration modules are installed and activated by default, even if the corresponding service is not installed on the machine.
			

SECURITY Changing the root password

				On the first login, identification is conducted with the root username and its usual password (or any user's login credentials which belongs to the sudo group). It is recommended to change the password used for webmin as soon as possible, so that if it is compromised, the server's user accounts will not be involved, even if this confers important administrative rights to the machine.
			
				Beware! Since webmin has so many features, a malicious user accessing it could compromise the security of the entire system. In general, interfaces of this kind are not recommended for important systems with strong security constraints (firewall, sensitive servers, etc.), and they are not recommended to be exposed to the public either.
			
				Webmin is used through a web interface, but it does not require Apache to be installed. Essentially, this software has its own integrated mini web server. This server listens by default on port 10000 and accepts secure HTTP connections.
			
				Included modules cover a wide variety of services, among which:
			
						all base services: creation of users and groups, management of crontab files, service scripts/files, viewing of logs, etc.
					
						bind: DNS server configuration (name service);
					
						postfix: SMTP server configuration (e-mail);
					
						network services: configuration of the xinetd super-server;
					
						disk quota: user quota management;
					
						dhcpd: DHCP server configuration;
					
						proftpd: FTP server configuration;
					
						samba: Samba file server configuration;
					
						software: installation or removal of software from Debian packages and system updates.
					
				The administration interface is available in a web browser at https://localhost:10000. Beware! Not all the modules are directly usable. Sometimes they must be configured by specifying the locations of the corresponding configuration files and some executable files (program). Frequently the system will politely prompt you when it fails to activate a requested module.
			

ALTERNATIVE GNOME control center

				The GNOME project also provides multiple administration interfaces that are usually accessible via the âSettingsâ entry in the user menu on the top right. gnome-control-center is the main program that brings them all together but many of the system wide configuration tools are effectively provided by other packages (accountsservice, system-config-printer, etc.). Although they are easy to use, these applications cover only a limited number of base services: user management, time configuration, network configuration, printer configuration, and so on.
			

### 9.4.2.Â Configuring Packages: debconf

				Many packages are automatically configured after asking a few questions during installation through the Debconf tool. These packages can be reconfigured by running dpkg-reconfigure -plevel package.
			
				For most cases, these settings are very simple; only a few important variables in the configuration file are changed. These variables are often grouped between two âdemarcationâ lines so that reconfiguration of the package only impacts the enclosed area. In other cases, reconfiguration will not change anything if the script detects a manual modification of the configuration file, in order to preserve these human interventions (because the script can't ensure that its own modifications will not disrupt the existing settings).
			

DEBIAN POLICY Preserving changes

				The Debian Policy expressly stipulates that everything should be done to preserve manual changes made to a configuration file, so more and more scripts take precautions when editing configuration files. The general principle is simple: the script will only make changes if it knows the status of the configuration file, which is verified by comparing the checksum of the file against that of the last automatically generated file. If they are the same, the script is authorized to change the configuration file. Otherwise, it determines that the file has been changed and asks what action it should take (install the new file, save the old file, or try to integrate the new changes with the existing file). This precautionary principle has long been unique to Debian, but other distributions have gradually begun to embrace it.
			
				The ucf program (from the Debian package of the same name) can be used to implement such a behavior.
			[Prev9.3.Â Managing Rights](sect.rights-management.en.md)[Up](#)[Home](index.en.md)[Next9.5.Â syslog System Events](sect.syslog.en.md)