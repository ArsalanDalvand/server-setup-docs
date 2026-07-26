---
title: "9.10.2. Restoring Machines without Backups"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.backup.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.backup.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.10.2. Restoring Machines without Backups"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.quotas.en.md)The Debian Administrator's Handbook[Next](sect.hotplug.en.md)

## 9.10.Â Backup

			Making backups is one of the main responsibilities of any administrator, but it is a complex subject, involving powerful tools which are often difficult to master.
		
			Many programs exist, such as amanda, bacula, or BackupPC. Those are client/server systems featuring many options, whose configuration is rather difficult. Some of them provide user-friendly web interfaces to mitigate this. For non-enterprise systems, administrators might want to check out rsnapshot or rdiff-backup. Users can easily create backups of their filesystems with timeshift, fsarchiver, duplicity, or even dd.
		
			Debian contains dozens of other backup software covering all possible use cases, as you can easily confirm with apt-cache search backup.
		
			Rather than detailing some of them, this section will present the thoughts of the Falcot Corp administrators when they defined their backup strategy.
		
			At Falcot Corp, backups have two goals: recovering erroneously deleted files, and quickly restoring any computer (server or desktop) whose hard drive has failed.
		

### 9.10.1.Â Backing Up with rsync

				Backups on tape having been deemed too slow and costly, data will be backed up on hard drives on a dedicated server, on which the use of software RAID (see [SectionÂ 12.1.1, âSoftware RAIDâ](advanced-administration.en.md#sect.raid-soft)) will protect the data from hard drive failure. Desktop computers are not backed up individually, but users are advised that their personal account on their department's file server will be backed up. The rsync command (from the package of the same name) is used daily to back up these different servers.
			

BACK TO BASICS The hard link, a second name for the file

				A hard link, as opposed to a symbolic link, cannot be differentiated from the linked file. Creating a hard link is essentially the same as giving an existing file a second name. This is why the deletion of a hard link only removes one of the names associated with the file. As long as another name is still assigned to the file, the data therein remain present on the filesystem. It is interesting to note that, unlike a copy, the hard link does not take up additional space on the hard drive.
			
				A hard link is created with the ln target link command. The link file is then a new name for the target file. Hard links can only be created on the same filesystem, while symbolic links are not subject to this limitation.
			
				The available hard drive space prohibits implementation of a complete daily backup. As such, the rsync command is preceded by a duplication of the content of the previous backup with hard links, which prevents usage of too much hard drive space. The rsync process then only replaces files that have been modified since the last backup. With this mechanism a great number of backups can be kept in a small amount of space. Since all backups are immediately available and accessible (for example, in different directories of a given share on the network), you can quickly make comparisons between two given dates.
			
				This backup mechanism is easily implemented with the dirvish program. It uses a backup storage space (âbankâ in its vocabulary) in which it places timestamped copies of sets of backup files (these sets are called âvaultsâ in the dirvish documentation).
			
				The main configuration is in the /etc/dirvish/master.conf file. It defines the location of the backup storage space, the list of âvaultsâ to manage, and default values for expiration of the backups. The rest of the configuration is located in the bank/vault/dirvish/default.conf files and contains the specific configuration for the corresponding set of files.
			

ExampleÂ 9.3.Â The /etc/dirvish/master.conf file

```bash
bank:
    /backup
exclude:
    lost+found/
    core
    *~
Runall:
    root    22:00
expire-default: +15 days
expire-rule:
#   MIN HR    DOM MON       DOW  STRFTIME_FMT
    *   *     *   *         1    +3 months
    *   *     1-7 *         1    +1 year
    *   *     1-7 1,4,7,10  1
```

				The bank setting indicates the directory in which the backups are stored. The exclude setting allows you to indicate files (or file types) to exclude from the backup. The Runall is a list of file sets to backup with a time-stamp for each set, which allows you to assign the correct date to the copy, in case the backup is not triggered at precisely the assigned time. You have to indicate a time just before the actual execution time (according to /etc/cron.d/dirvish). Finally, the expire-default and expire-rule settings define the expiration policy for backups. The above example keeps forever backups that are generated on the first Sunday of each quarter, deletes after one year those from the first Sunday of each month, and after 3 months those from other Sundays. Other daily backups are kept for 15 days. The order of the rules does matter, Dirvish uses the last matching rule, or the expire-default one if no other expire-rule matches.
			

IN PRACTICE Scheduled expiration

				The expiration rules are not used by dirvish-expire to do its job. In reality, the expiration rules are applied when creating a new backup copy to define the expiration date associated with that copy. dirvish-expire simply peruses the stored copies and deletes those for which the expiration date has passed.
			

ExampleÂ 9.4.Â The /backup/root/dirvish/default.conf file

```bash
client: rivendell.falcot.com
tree: /
xdev: 1
index: gzip
image-default: %Y%m%d
exclude:
    /var/cache/apt/archives/*.deb
    /var/cache/man/**
    /tmp/**
    /var/tmp/**
    *.bak
```

				The above example specifies the set of files to back up: these are files on the machine rivendell.falcot.com (for local data backup, simply specify the name of the local machine as indicated by hostname), especially those in the root tree (tree: /), except those listed in exclude. The backup will be limited to the contents of one filesystem (xdev: 1). It will not include files from other mount points. An index of saved files will be generated (index: gzip), and the image will be named according to the current date (image-default: %Y%m%d).
			
				There are many options available, all documented in the dirvish.conf(5) manual page. Once these configuration files are setup, you have to initialize each file set with the dirvish --vault vault --init command. From there on the daily invocation of dirvish-runall will automatically create a new backup copy just after having deleted those that expired.
			

IN PRACTICE Remote backup over SSH

				When dirvish needs to save data to a remote machine, it will use ssh to connect to it, and will start rsync as a server. This requires the root user to be able to automatically connect to it. The use of an SSH authentication key allows precisely that (see [SectionÂ 9.2.1.1, âKey-Based Authenticationâ](sect.remote-login.en.md#sect.ssh-key-based-auth)).
			

### 9.10.2.Â Restoring Machines without Backups

				Desktop computers, which are not backed up, will be easy to reinstall from custom DVD-ROMs/USB sticks prepared with simple-cdd (see [SectionÂ 12.3.3, âSimple-CDD: The All-In-One Solutionâ](sect.automated-installation.en.md#sect.simple-cdd)). Since this performs an installation from scratch, it loses any customization that can have been made after the initial installation. This is fine since the systems are all hooked to a central LDAP directory for accounts and most desktop applications are preconfigured thanks to dconf (see [SectionÂ 13.3.1, âGNOMEâ](sect.graphical-desktops.en.md#sect.gnome-desktop) for more information about this).
			
				The Falcot Corp administrators are aware of the limits in their backup policy. Since they can't protect the backup server as well as a tape in a fireproof safe, they have installed it in a separate room so that a disaster such as a fire in the server room won't destroy backups along with everything else. Furthermore, they do an incremental backup on DVD-ROM once per week â only files that have been modified since the last backup are included.
			

GOING FURTHER Backing up SQL and LDAP services

				Many services (such as SQL or LDAP databases) cannot be backed up by simply copying their files (unless they are properly interrupted during creation of the backups, which is frequently problematic, since they are intended to be available at all times). As such, it is necessary to use an âexportâ mechanism to create a âdata dumpâ that can be safely backed up. These are often quite large, but they compress well. To reduce the storage space required, you will only store a complete text file per week, and a diff each day, which is created with a command of the type diff file_from_yesterday file_from_today. The xdelta program produces incremental differences from binary dumps.
			

CULTURE TAR, the standard for tape backups

				Historically, the simplest means of making a backup on Unix was to store a TAR archive on a tape. The tar command even got its name from âTape ARchiveâ.
			[Prev9.9.Â Quotas](sect.quotas.en.md)[Up](#)[Home](index.en.md)[Next9.11.Â Hot Plugging: hotplug](sect.hotplug.en.md)