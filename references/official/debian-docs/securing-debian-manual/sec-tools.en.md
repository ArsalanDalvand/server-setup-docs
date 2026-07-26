---
title: "8.1. Remote vulnerability assessment tools"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/sec-tools.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/sec-tools.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "8.1. Remote vulnerability assessment tools"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](deb-pack-sign.en.md)Securing Debian Manual[Next](ch08s02.en.md)

# ChapterÂ 8.Â Security tools in Debian

[8.1. Remote vulnerability assessment tools](sec-tools.en.md#vuln-asses)[8.2. Network scanner tools](ch08s02.en.md)[8.3. Internal audits](ch08s03.en.md)[8.4. Auditing source code](ch08s04.en.md)[8.5. Virtual Private Networks](vpn.en.md)[8.5.1. Point to Point tunneling](vpn.en.md#idm3716)[8.6. Public Key Infrastructure (PKI)](ch08s06.en.md)[8.7. SSL Infrastructure](ch08s07.en.md)[8.8. Antivirus tools](ch08s08.en.md)[8.9. GPG agent](gpg-agent.en.md)
		FIXME: More content needed.
	
		Debian provides also a number of security tools that can make a Debian box suited for security purposes. These purposes include protection of information systems through firewalls (either packet or application-level), intrusion detection (both network and host based), vulnerability assessment, antivirus, private networks, etc.
	
		Since Debian 3.0 (woody), the distribution features cryptographic software integrated into the main distribution. OpenSSH and GNU Privacy Guard are included in the default install, and strong encryption is now present in web browsers and web servers, databases, and so forth. Further integration of cryptography is planned for future releases. This software, due to export restrictions in the US, was not distributed along with the main distribution but included only in non-US sites.
	

## 8.1.Â Remote vulnerability assessment tools

			The tools provided by Debian to perform remote vulnerability assessment are: [[55]](#ftn.idm3583)

nessus

raccess

nikto (whisker's replacement)
					

			By far, the most complete and up-to-date tools is nessus which is composed of a client (nessus) used as a GUI and a server (nessusd) which launches the programmed attacks. Nessus includes remote vulnerabilities for quite a number of systems including network appliances, ftp servers, www servers, etc. The latest security plugins are able even to parse a web site and try to discover which interactive pages are available which could be attacked. There are also Java and Win32 clients (not included in Debian) which can be used to contact the management server.
		
nikto is a web-only vulnerability assessment scanner including anti-IDS tactics (most of which are not anti-IDS anymore). It is one of the best cgi-scanners available, being able to detect a WWW server and launch only a given set of attacks against it. The database used for scanning can be easily modified to provide for new information.
		[[55]](#idm3583)
				Some of them are provided when installing the harden-remoteaudit package.
			[Prev7.5.Â Package signing in Debian](deb-pack-sign.en.md)[Up](#)[Home](index.en.md)[Next8.2.Â Network scanner tools](ch08s02.en.md)