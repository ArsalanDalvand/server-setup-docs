---
title: "4.19. Taking a snapshot of the system"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/snapshot.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/snapshot.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.19. Taking a snapshot of the system"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](network-secure.en.md)Securing Debian Manual[Next](ch04s20.en.md)

## 4.19.Â Taking a snapshot of the system

			Before putting the system into production system you could take a snapshot of the whole system. This snapshot could be used in the event of a compromise (see [ChapterÂ 11, After the compromise (incident response)](after-compromise.en.md)). You should remake this upgrade whenever the system is upgraded, especially if you upgrade to a new Debian release.
		
			For this you can use a writable removable-media that can be set up read-only, this could be a floppy disk (read protected after use), a CD on a CD-ROM unit (you could use a rewritable CD-ROM so you could even keep backups of md5sums in different dates), or a USB disk or MMC card (if your system can access those and they can be write protected).
		
			The following script creates such a snapshot: 

```bash
#!/bin/bash
/bin/mount /dev/fd0 /mnt/floppy
trap "/bin/umount /dev/fd0" 0 1 2 3 9 13 15
if [ ! -f /usr/bin/md5sum ] ; then
	echo "Cannot find md5sum. Aborting."
	exit 1
fi
/bin/cp /usr/bin/md5sum /mnt/floppy
echo "Calculating md5 database"
>/mnt/floppy/md5checksums.txt
for dir in /bin/ /sbin/ /usr/bin/ /usr/sbin/ /lib/ /usr/lib/
do
   find $dir -type f | xargs /usr/bin/md5sum >>/mnt/floppy/md5checksums-lib.txt
done
echo "post installation md5 database calculated"
if [ ! -f /usr/bin/sha1sum ] ; then
	echo "Cannot find sha1sum"
        echo "WARNING: Only md5 database will be stored"
else
	/bin/cp /usr/bin/sha1sum /mnt/floppy
	echo "Calculating SHA-1 database"
	>/mnt/floppy/sha1checksums.txt
	for dir in /bin/ /sbin/ /usr/bin/ /usr/sbin/ /lib/ /usr/lib/
	do
	   find $dir -type f | xargs /usr/bin/sha1sum >>/mnt/floppy/sha1checksums-lib.txt
	done
	echo "post installation sha1 database calculated"
fi
exit 0
```

			Note that the md5sum binary (and sha1sum, if available) is placed on the floppy drive so it can be used later on to check the binaries of the system (just in case it gets trojaned). However, if you want to make sure that you are running a legitimate binary, you might want to either compile a static copy of the md5sum binary and use that one (to prevent a trojaned libc library from interfering with the binary) or to use the snapshot of md5sums only from a clean environment such as a rescue CD-ROM or a Live-CD (to prevent a trojaned kernel from interfering). I cannot stress this enough: if you are on a compromised system you cannot trust its output, see [ChapterÂ 11, After the compromise (incident response)](after-compromise.en.md).
		
			The snapshot does not include the files under /var/lib/dpkg/info which includes the MD5 hashes of installed packages (in files ending with .md5sums). You could copy this information along too, however you should notice:
		

						the md5sums files include the md5sum of all files provided by the Debian packages, not just system binaries. As a consequence, that database is bigger (5 Mb versus 600 Kb in a Debian GNU/Linux system with a graphical system and around 2.5 Gb of software installed) and will not fit in small removable media (like a single floppy disk, but would probably fit in a removable USB memory).
					
						not all Debian packages provide md5sums for the files installed since it is not (currently) mandated policy. Notice, however, that you can generate the md5sums for all packages using debsums after you've finished the system installation: 

```bash
# debsums --generate=missing,keep
```

			Once the snapshot is done you should make sure to set the medium read-only. You can then store it for backup or place it in the drive and use it to drive a cron check nightly comparing the original md5sums against those on the snapshot.
		
			If you do not want to setup a manual check you can always use any of the integrity systems available that will do this and more, for more information please read [SectionÂ 10.2, âDo periodic integrity checksâ](periodic-integrity.en.md).
		[Prev4.18.Â Securing network access](network-secure.en.md)[Up](#)[Home](index.en.md)[Next4.20.Â Other recommendations](ch04s20.en.md)