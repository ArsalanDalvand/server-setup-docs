---
title: "11.3. FTP File Server"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.ftp-file-server.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.ftp-file-server.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "11.3. FTP File Server"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.http-web-server.en.md)The Debian Administrator's Handbook[Next](sect.nfs-file-server.en.md)

## 11.3.Â FTP File Server

			FTP (File Transfer Protocol) is one of the first protocols of the Internet (RFCÂ 959 was issued in 1985!). It was used to distribute files before the Web was even born (the HTTP protocol was created in 1990, and formally defined in its 1.0 version by RFCÂ 1945, issued in 1996).
		
			This protocol allows both file uploads and file downloads; for this reason, it is still widely used to deploy updates to a website hosted by one's Internet service provider (or any other entity hosting websites). In these cases, access is enforced with a user identifier and password; on successful authentication, the FTP server grants read-write access to that user's home directory.
		

SECURITY FTP, FTPS, and SFTP

			FTP is not a secure protocol. User names, passwords, and data are transported in clear text. To increase the security, one can use FTP via implicit (enforced) or explicit (requested) SSL/TLS, also known as FTPS, but not to be confused with SFTP, the SSH File Transfer Protocol, an alternative file transfer implementation using SSH. Many FTP server packages in Debian support FTPS.
		
			Other FTP servers are mainly used to distribute files for public downloading; Debian packages are a good example. The contents of these servers is fetched from other, geographically remote, servers; it is then made available to less distant users. This means that client authentication is not required; as a consequence, this operating mode is known as âanonymous FTPâ. To be perfectly correct, the clients do authenticate with the anonymous username; the password is often, by convention, the user's email address, but the server ignores it.
		
			Many FTP servers are available in Debian (ftpd(-ssl), proftpd-basic, pure-ftpd and so on), which all provide the virtual ftp-server package. Please note that the pyftpd package, however, has been removed from the Debian project due to not being actively maintained anymore and being incompatible with Python 3. The Falcot Corp administrators picked vsftpd because they only use the FTP server to distribute a few files (including a Debian package repository); since they don't need advanced features, they chose to focus on the security aspects.
		
			Installing the package creates an ftp system user. This account is always used for anonymous FTP connections, and its home directory (/srv/ftp/) is the root of the tree made available to users connecting to this service. The default configuration (in /etc/vsftpd.conf) requires some changes to cater to the simple need of making big files available for public downloads: anonymous access needs to be enabled (anonymous_enable=YES) and read-only access of local users needs to be disabled (local_enable=NO). The latter is particularly important since the FTP protocol doesn't use any form of encryption and the user password could be intercepted over the wire.
		
			FTP and FTPS resources can be accessed with a variety of clients, with and without a graphical user interface, and they can also be mounted locally using the curlftpfs command from the similarly named package.
		[Prev11.2.Â Web Server (HTTP)](sect.http-web-server.en.md)[Up](#)[Home](index.en.md)[Next11.4.Â NFS File Server](sect.nfs-file-server.en.md)