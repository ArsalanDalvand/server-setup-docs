---
title: "B.1.5. System Information: Memory, Disk Space, Identity"
source: "https://www.debian.org/doc/manuals/debian-handbook/short-remedial-course.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/short-remedial-course.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "B.1.5. System Information: Memory, Disk Space, Identity"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.other-derivatives.en.md)The Debian Administrator's Handbook[Next](sect.filesystem-hierarchy.en.md)

# AppendixÂ B.Â Short Remedial Course

		Even though this book primarily targets administrators and âpower-usersâ, we wouldn't like to exclude motivated beginners. This appendix will therefore be a crash-course describing the fundamental concepts involved in handling a Unix computer.
	

## B.1.Â Shell and Basic Commands

			In the Unix world, every administrator has to use the command line sooner or later; for example, when the system fails to start properly and only provides a command-line rescue mode. Being able to handle such an interface, therefore, is a basic survival skill for these circumstances.
		

QUICK LOOK Starting the command interpreter

			A command-line environment can be run from the graphical desktop, by an application known as a âterminalâ. In GNOME, you can start it from the âActivitiesâ overview (that you get when you move the mouse in the top-left corner of the screen) by typing the first letters of the application name. In Plasma (and many other desktop variants), you will find it in the applications menu Applications â System menu.
		
			This section only gives a quick peek at the commands. They all have many options not described here, so please refer to the abundant documentation in their respective manual pages.
		

### B.1.1.Â Browsing the Directory Tree and Managing Files

				Once a session is open, the pwd command (which stands for print working directory) displays the current location in the filesystem. The current directory is changed with the cd directory command (cd is for change directory). The parent directory is always called .. (two dots), whereas the current directory is also known as . (one dot). The ls command allows listing the contents of a directory. If no parameters are given, it operates on the current directory.
			

```bash
$ pwd
/home/rhertzog
$ cd Desktop
$ pwd
/home/rhertzog/Desktop
$ cd .
$ pwd
/home/rhertzog/Desktop
$ cd ..
$ pwd
/home/rhertzog
$ ls
Desktop    Downloads  Pictures  Templates
Documents  Music      Public    Videos
```

				A new directory can be created with mkdir directory, and an existing (empty) directory can be removed with rmdir directory. The mv command allows moving and/or renaming files and directories; removing a file is achieved with rm file.
			

```bash
$ mkdir test
$ ls
Desktop    Downloads  Pictures  Templates  Videos
Documents  Music      Public    test
$ mv test new
$ ls
Desktop    Downloads  new       Public     Videos
Documents  Music      Pictures  Templates
$ rmdir new
$ ls
Desktop    Downloads  Pictures  Templates  Videos
Documents  Music      Public
```

### B.1.2.Â Displaying and Modifying Text Files

				The cat file command (intended to concatenate files to the standard output device) reads a file and displays its contents on the terminal. If the file is too big to fit on a screen, use a pager such as less (or more) to display it page by page.
			
				The editor command starts a text editor (such as vi or nano) and allows creating, modifying and reading text files. The simplest files can sometimes be created directly from the command interpreter thanks to redirection: echo "text" >file creates a file named file with âtextâ as its contents. Adding a line at the end of this file is possible too, with a command such as echo "moretext" >>file. Note the >> in this example.
			

### B.1.3.Â Searching for Files and within Files

				The find directory criteria command looks for files in the hierarchy under directory according to several criteria. The most commonly used criterion is -name name: that allows looking for a file by its name.
			
				The grep expression files command searches the contents of the files and extracts the lines matching the regular expression (see sidebar [BACK TO BASICS Regular expression](network-services.en.md#sidebar.regexp)). Adding the -r option enables a recursive search on all files contained in the directory passed as a parameter. This allows looking for a file when only a part of the contents are known.
			

### B.1.4.Â Managing Processes

				The ps aux command lists the processes currently running and helps identifying them by showing their pid (process id). Once the pid of a process is known, the kill -signal pid command allows sending it a signal (if the process belongs to the current user). Several signals exist; most commonly used are TERM (a request to terminate gracefully) and KILL (a forced kill).
			
				The command interpreter can also run programs in the background if the command is followed by a â&â. By using the ampersand, the user resumes control of the shell immediately even though the command is still running (hidden from the user; as a background process). The jobs command lists the processes running in the background; running fg %job-number (for foreground) restores a job to the foreground. When a command is running in the foreground (either because it was started normally, or brought back to the foreground with fg), the Control+Z key combination pauses the process and resumes control of the command-line. The process can then be restarted in the background with bg %job-number (for background).
			

### B.1.5.Â System Information: Memory, Disk Space, Identity

				The free command displays information on memory; df (disk free) reports on the available disk space on each of the disks mounted in the filesystem. Its -h option (for human readable) converts the sizes into a more legible unit (usually mebibytes or gibibytes). In a similar fashion, the free command supports the -m and -g options, and displays its data either in mebibytes or in gibibytes, respectively.
			

```bash
$ free
              total        used        free      shared  buff/cache   available
Mem:       16279260     5910248      523432      871036     9845580     9128964
Swap:      16601084      240640    16360444
$ df
Filesystem                1K-blocks      Used Available Use% Mounted on
udev                        8108516         0   8108516   0% /dev
tmpfs                       1627928    161800   1466128  10% /run
/dev/mapper/vg_main-root  466644576 451332520  12919912  98% /
tmpfs                       8139628    146796   7992832   2% /dev/shm
tmpfs                          5120         4      5116   1% /run/lock
tmpfs                       8139628         0   8139628   0% /sys/fs/cgroup
/dev/sda1                    523248      1676    521572   1% /boot/efi
tmpfs                       1627924        88   1627836   1% /run/user/1000
```

				The id command displays the identity of the user running the session, along with the list of groups they belong to. Since access to some files or devices may be limited to group members, checking available group membership may be useful.
			

```bash
$ id
uid=1000(rhertzog) gid=1000(rhertzog) groups=1000(rhertzog),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),108(netdev),109(bluetooth),115(scanner)
```

[PrevA.14.Â And Many More](sect.other-derivatives.en.md)[Up](#)[Home](index.en.md)[NextB.2.Â Organization of the Filesystem Hierarchy](sect.filesystem-hierarchy.en.md)