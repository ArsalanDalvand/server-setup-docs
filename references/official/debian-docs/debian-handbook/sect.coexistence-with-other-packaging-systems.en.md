---
title: "5.5. Coexistence with Other Packaging Systems"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.coexistence-with-other-packaging-systems.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.coexistence-with-other-packaging-systems.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "5.5. Coexistence with Other Packaging Systems"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.manipulating-packages-with-dpkg.en.md)The Debian Administrator's Handbook[Next](apt.en.md)

## 5.5.Â Coexistence with Other Packaging Systems

			Debian packages are not the only software packages used in the free software world. The main competitor is the RPM format of the Red Hat Linux distribution and its many derivatives. Red Hat is a very popular, commercial distribution. It is thus common for software provided by third parties to be offered as RPM packages rather than Debian.
		
			In this case, you should know that the program rpm, which handles RPM packages, is available as a Debian package, so it is possible to use this package format on Debian. Care should be taken, however, to limit these manipulations to extract the information from a package or to verify its integrity. It is, in truth, unreasonable to use rpm to install an RPM on a Debian system; RPM uses its own database, separate from those of native software (such as dpkg). This is why it is not possible to ensure a stable coexistence of two packaging systems.
		
			On the other hand, the alien utility can convert RPM packages into Debian packages, and vice versa.
		

COMMUNITY Encouraging the adoption of .deb

			If you regularly use the alien program to install RPM packages coming from one of your providers, do not hesitate to write to them and amicably express your strong preference for the .deb format. Note that the format of the package is not everything: a .deb package built with alien or prepared for a version of Debian different than that which you use, or even for a derivative distribution like Ubuntu, would probably not offer the same level of quality and integration as a package specifically developed for Debian Bullseye.
		

```bash
$ fakeroot alien --to-deb phpMyAdmin-5.1.1-2.fc35.noarch.rpm
[..]
Warning: Skipping conversion of scripts in package phpMyAdmin: postinst
Warning: Use the --scripts parameter to include the scripts.
[..]
phpmyadmin_5.1.1-3_all.deb generated
$ ls -sh phpmyadmin_5.1.1-3_all.deb
  6,0M phpmyadmin_5.1.1-3_all.deb
$ dpkg -c phpmyadmin_5.1.1-3_all.deb
drwxr-xr-x root/root         0 2021-08-09 02:02 ./
drwxr-xr-x root/root         0 2021-08-09 02:02 ./etc/
drwxr-xr-x root/root         0 2021-08-09 02:02 ./etc/httpd/
drwxr-xr-x root/root         0 2021-08-09 02:02 ./etc/httpd/conf.d/
-rw-r--r-- root/root      1181 2021-07-27 09:32 ./etc/httpd/conf.d/phpMyAdmin.conf
drwxr-xr-x root/root         0 2021-08-09 02:02 ./etc/nginx/
drwxr-xr-x root/root         0 2021-08-09 02:02 ./etc/nginx/default.d/
-rw-r--r-- root/root       430 2021-07-27 09:32 ./etc/nginx/default.d/phpMyAdmin.conf
drwxr-x--- root/root         0 2021-08-09 02:02 ./etc/phpMyAdmin/
-rw-r----- root/root      4546 2021-07-27 09:34 ./etc/phpMyAdmin/config.inc.php
drwxr-xr-x root/root         0 2021-08-09 02:02 ./usr/
drwxr-xr-x root/root         0 2021-08-09 02:02 ./usr/share/
drwxr-xr-x root/root         0 2021-08-09 02:02 ./usr/share/doc/
drwxr-xr-x root/root         0 2021-08-09 02:02 ./usr/share/doc/phpMyAdmin/
[..]
$ dpkg -I phpmyadmin_5.1.1-3_all.deb
 new Debian package, version 2.0.
 size 6195324 bytes: control archive=44444 bytes.
     102 bytes,     3 lines      conffiles
     593 bytes,    15 lines      control
  180405 bytes,  1919 lines      md5sums
     448 bytes,    11 lines   *  postinst             #!/bin/sh
 Package: phpmyadmin
 Version: 5.1.1-3
 Architecture: all
 Maintainer: Daniel Leidert <dleidert@debian.org>
 Installed-Size: 40693
 Section: alien
 Priority: extra
 Description: A web interface for MySQL and MariaDB
  phpMyAdmin is a tool written in PHP intended to handle the administration of
  MySQL over the Web. Currently it can create and drop databases,
  create/drop/alter tables, delete/edit/add fields, execute any SQL statement,
  manage keys on fields, manage privileges,export data into various formats and
  is available in 50 languages
  .
  (Converted from a rpm package by alien version 8.95.4.)
```

			You will find that this process is extremely simple. You must know, however, that the package generated does not have any dependency information, since the dependencies in the two packaging formats don't have systematic correspondence. The administrator must thus manually ensure that the converted package will function correctly, and this is why Debian packages thus generated should be avoided as much as possible. Fortunately, Debian has the largest collection of software packages of all distributions, and it is likely that whatever you seek is already in there.
		
			Looking at the man page for the alien command, you will also note that this program handles other packaging formats, especially the one used by the Slackware distribution (it is made of a simple tar.gz archive).
		
			The stability of the software deployed using the dpkg tool contributes to Debian's fame. The APT suite of tools, described in the following chapter, preserves this advantage, while relieving the administrator from managing the status of packages, a necessary but difficult task.
		[Prev5.4.Â Manipulating Packages with dpkg](sect.manipulating-packages-with-dpkg.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 6.Â Maintenance and Updates: The APT Tools](apt.en.md)