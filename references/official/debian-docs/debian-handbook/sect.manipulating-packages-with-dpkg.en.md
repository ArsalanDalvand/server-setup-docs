---
title: "5.4.5. Multi-Arch Support"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.manipulating-packages-with-dpkg.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.manipulating-packages-with-dpkg.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "5.4.5. Multi-Arch Support"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.source-package-structure.en.md)The Debian Administrator's Handbook[Next](sect.coexistence-with-other-packaging-systems.en.md)

## 5.4.Â Manipulating Packages with dpkg

dpkg is the base command for handling Debian packages on the system. If you have .deb packages, it is dpkg that allows installation or analysis of their contents. But this program only has a partial view of the Debian universe: it knows what is installed on the system, and whatever it is given on the command line, but knows nothing of the other available packages. As such, it will fail if a dependency is not met. Tools such as apt and aptitude, on the contrary, will create a list of dependencies to install everything as automatically as possible.
		

NOTE dpkg or apt?

dpkg should be seen as a system tool (backend), and apt as a tool closer to the user, which overcomes the limitations of the former. These tools work together, each one with its particularities, suited to specific tasks.
		

### 5.4.1.Â Installing Packages

dpkg is, above all, the tool for installing an already available Debian package (because it does not download anything). To do this, we use its -i or --install option.
			

ExampleÂ 5.2.Â Installation of a package with dpkg

```bash
# dpkg -i man-db_2.9.4-2_amd64.deb
(Reading database ... 227466 files and directories currently installed.)
Preparing to unpack man-db_2.9.4-2_amd64.deb ...
Unpacking man-db (2.9.4-2) over (2.8.5-2) ...
Setting up man-db (2.9.4-2) ...
Updating database of manual pages ...
man-db.service is a disabled or a static unit not running, not starting it.
Processing triggers for mailcap (3.69) ...
```

				We can see the different steps performed by dpkg; we know, thus, at what point any error may have occurred. The installation can also be effected in two stages: first unpacking, then configuration. apt takes advantage of this, limiting the number of calls to dpkg (since each call is costly, due to loading of the database in memory, especially the list of already installed files).
			

ExampleÂ 5.3.Â Separate unpacking and configuration

```bash
# dpkg --unpack man-db_2.9.4-2_amd64.deb
(Reading database ... 227466 files and directories currently installed.)
Preparing to unpack man-db_2.9.4-2_amd64.deb ...
Unpacking man-db (2.9.4-2) over (2.9.4-2) ...
Processing triggers for mailcap (3.69) ...
# dpkg --configure man-db
Setting up man-db (2.9.4-2) ...
Updating database of manual pages ...
man-db.service is a disabled or a static unit not running, not starting it.
```

				Sometimes dpkg will fail to install a package and return an error; if the user orders it to ignore this, it will only issue a warning; it is for this reason that we have the different --force-* options. The dpkg --force-help command, or documentation of this command, will give a complete list of these options. The most frequent error, which you are bound to encounter sooner or later, is a file collision. When a package contains a file that is already installed by another package, dpkg will refuse to install it. The following messages will then appear:
			

```bash
Unpacking libgdm (from .../libgdm_3.8.3-2_amd64.deb) ...
dpkg: error processing /var/cache/apt/archives/libgdm_3.8.3-2_amd64.deb (--unpack):
 trying to overwrite '/usr/bin/gdmflexiserver', which is also in package gdm3 3.4.1-9
```

				In this case, if you think that replacing this file is not a significant risk to the stability of your system (which is usually the case), you can use the option --force-overwrite, which tells dpkg to ignore this error and overwrite the file.
			
				While there are many available --force-* options, only --force-overwrite is likely to be used regularly. These options only exist for exceptional situations, rarely encountered in Debian Stable. It is better to leave them alone as much as possible in order to respect the rules imposed by the packaging mechanism. Do not forget, these rules ensure the consistency and stability of your system.
			

CAUTION Effective use of --force-*

				If you are not careful, the use of an option --force-* can lead to a system where the APT family of commands will refuse to function. In effect, some of these options allow installation of a package when a dependency is not met, or when there is a conflict. The result is an inconsistent system from the point of view of dependencies, and the APT commands will refuse to execute any action except those that will bring the system back to a consistent state (this often consists of installing the missing dependency or removing a problematic package). This often results in a message like this one, obtained after installing a new version of rdesktop while ignoring its dependency on a newer version of the libc6:
			

```bash
# apt full-upgrade
[...]
You might want to run 'apt-get -f install' to correct these.
The following packages have unmet dependencies:
  rdesktop: Depends: libc6 (>= 2.5) but 2.3.6.ds1-13etch7 is installed
E: Unmet dependencies. Try using -f.
```

				A courageous administrator who is certain of the correctness of their analysis may choose to ignore a dependency or conflict and use the corresponding --force-* option. In this case, if they want to be able to continue to use apt or aptitude, they must edit /var/lib/dpkg/status to delete/modify the dependency, or conflict, that they chose to override.
			
				This manipulation is an ugly hack, and should never be used, except in the most extreme case of necessity. Quite frequently, a more fitting solution is to recompile the package that is causing the problem (see [SectionÂ 15.1, âRebuilding a Package from its Sourcesâ](debian-packaging.en.md#sect.rebuilding-package)) or use a new version (potentially corrected) from a repository such as the stable-backports one (see [SectionÂ 6.1.2.4, âStable Backportsâ](apt.en.md#sect.apt-sources.list.stable.backports)).
			

### 5.4.2.Â Package Removal

				Invoking dpkg with the -r or --remove option, followed by the name of a package, removes that package. This removal is, however, not complete: all of the configuration files, maintainer scripts, log files (system logs) and other user data handled by the package remain. That way disabling the program is easily done by uninstalling it, and it is still possible to quickly reinstall it with the same configuration. To completely remove everything associated with a package, use the -P or --purge option, followed by the package name.
			

ExampleÂ 5.4.Â Removal and purge of the debian-cd package

```bash
# dpkg -r debian-cd
(Reading database ... 228705 files and directories currently installed.)
Removing debian-cd (3.1.35) ...
# dpkg -P debian-cd
(Reading database ... 228049 files and directories currently installed.)
Purging configuration files for debian-cd (3.1.35) ...
```

### 5.4.3.Â Querying dpkg's Database and Inspecting .deb Files

BACK TO BASICS Option syntax

				Most options are available in a âlongâ version (one or more relevant words, preceded by a double dash) and a âshortâ version (a single letter, often the initial of one word from the long version, and preceded by a single dash). This convention is so common that it is a POSIX standard.
			
				Before concluding this section, we will study dpkg options that query the internal database in order to obtain information. Giving first the long options and then corresponding short options (that will evidently take the same possible arguments) we cite
			
--listfiles package (or -L), which lists the files installed by this package;
					
--search file (or -S), which finds the package(s) containing the file;
					
--status package (or -s), which displays the headers of an installed package;
					
--list (or -l), which displays the list of packages known to the system and their installation status;
					
--contents file.deb (or -c), which lists the files in the Debian package specified;
					
--info file.deb  (or -I), which displays the headers of this Debian package.
					

CAUTION dpkg --search and merged /usr

				For [various reasons](https://www.freedesktop.org/wiki/Software/systemd/TheCaseForTheUsrMerge/), Debian now installs by default a few top-level directories as symlinks to their counterparts below /usr. For instance, /bin, /sbin and /lib are now symlinks to, respectively, /usr/bin, /usr/sbin and /usr/lib.
			
				While this does provide desirable benefits, it can also be a source of confusion. For example, when you query dpkg which package is owning a given file, it will only be able to answer when you ask for its original path:
			

```bash
$ dpkg --search /bin/mount
mount: /bin/mount
$ dpkg --search /usr/bin/mount
dpkg-query: no path found matching pattern /usr/bin/mount
$ dpkg --search /bin/apt
dpkg-query: no path found matching pattern /bin/apt
$ dpkg --search /usr/bin/apt
apt: /usr/bin/apt
```

				This issue is currently tracked as bug #858331. There is also an ongoing discussion if the approach used so far is counterproductive. âÂ [https://bugs.debian.org/858331](https://bugs.debian.org/858331)

ExampleÂ 5.5.Â Various queries with dpkg

```bash
$ dpkg -L base-passwd
/.
/usr
/usr/sbin
/usr/sbin/update-passwd
/usr/share
/usr/share/base-passwd
/usr/share/base-passwd/group.master
/usr/share/base-passwd/passwd.master
/usr/share/doc
/usr/share/doc/base-passwd
/usr/share/doc/base-passwd/README
/usr/share/doc/base-passwd/changelog.gz
/usr/share/doc/base-passwd/copyright
/usr/share/doc/base-passwd/users-and-groups.html
/usr/share/doc/base-passwd/users-and-groups.txt.gz
/usr/share/doc-base
/usr/share/doc-base/users-and-groups
/usr/share/lintian
/usr/share/lintian/overrides
/usr/share/lintian/overrides/base-passwd
/usr/share/man
/usr/share/man/de
/usr/share/man/de/man8
/usr/share/man/de/man8/update-passwd.8.gz
/usr/share/man/es
/usr/share/man/es/man8
/usr/share/man/es/man8/update-passwd.8.gz
/usr/share/man/fr
/usr/share/man/fr/man8
/usr/share/man/fr/man8/update-passwd.8.gz
/usr/share/man/ja
/usr/share/man/ja/man8
/usr/share/man/ja/man8/update-passwd.8.gz
/usr/share/man/man8
/usr/share/man/man8/update-passwd.8.gz
/usr/share/man/pl
/usr/share/man/pl/man8
/usr/share/man/pl/man8/update-passwd.8.gz
/usr/share/man/ru
/usr/share/man/ru/man8
/usr/share/man/ru/man8/update-passwd.8.gz
$ dpkg -S /bin/date
coreutils: /bin/date
$ dpkg -s coreutils
Package: coreutils
Essential: yes
Status: install ok installed
Priority: required
Section: utils
Installed-Size: 17478
Maintainer: Michael Stone <mstone@debian.org>
Architecture: amd64
Multi-Arch: foreign
Source: coreutils (8.32-4)
Version: 8.32-4+b1
Pre-Depends: libacl1 (>= 2.2.23), libattr1 (>= 1:2.4.44), libc6 (>= 2.28), libgmp10, libselinux1 (>= 3.1~)
Description: GNU core utilities
 This package contains the basic file, shell and text manipulation
 utilities which are expected to exist on every operating system.
 .
 Specifically, this package includes:
 arch base64 basename cat chcon chgrp chmod chown chroot cksum comm cp
 csplit cut date dd df dir dircolors dirname du echo env expand expr
 factor false flock fmt fold groups head hostid id install join link ln
 logname ls md5sum mkdir mkfifo mknod mktemp mv nice nl nohup nproc numfmt
 od paste pathchk pinky pr printenv printf ptx pwd readlink realpath rm
 rmdir runcon sha*sum seq shred sleep sort split stat stty sum sync tac
 tail tee test timeout touch tr true truncate tsort tty uname unexpand
 uniq unlink users vdir wc who whoami yes
Homepage: http://gnu.org/software/coreutils
$ dpkg -l 'b*'
Desired=Unknown/Install/Remove/Purge/Hold
| Status=Not/Inst/Conf-files/Unpacked/halF-conf/Half-inst/trig-aWait/Trig-pend
|/ Err?=(none)/Reinst-required (Status,Err: uppercase=bad)
||/ Name                       Version       Architecture Description
+++-==========================-==============-============-==================================
un  backupninja                <none>         <none>       (no description available)
un  backuppc                   <none>         <none>       (no description available)
ii  baloo-kf5                  5.78.0-3       amd64        framework for searching and manag>
un  balsa                      <none>         <none>       (no description available)
ii  baobab                     3.38.0-1       amd64        GNOME disk usage analyzer
un  base                       <none>         <none>       (no description available)
un  base-config                <none>         <none>       (no description available)
ii  base-files                 11.1           amd64        Debian base system miscellaneous >
ii  base-passwd                3.5.51         amd64        Debian base system master passwor>
ii  bash                       5.1-2+b1       amd64        GNU Bourne Again SHell
[..]
$ dpkg -c /var/cache/apt/archives/bash_5.1-3+b1_amd64.deb
drwxr-xr-x root/root         0 2021-07-25 20:43 ./
drwxr-xr-x root/root         0 2021-07-25 20:43 ./bin/
-rwxr-xr-x root/root   1234376 2021-07-25 20:43 ./bin/bash
drwxr-xr-x root/root         0 2021-07-25 20:43 ./etc/
-rw-r--r-- root/root      1994 2021-07-25 20:43 ./etc/bash.bashrc
drwxr-xr-x root/root         0 2021-07-25 20:43 ./etc/skel/
-rw-r--r-- root/root       220 2021-07-25 20:43 ./etc/skel/.bash_logout
-rw-r--r-- root/root      3526 2021-07-25 20:43 ./etc/skel/.bashrc
-rw-r--r-- root/root       807 2021-07-25 20:43 ./etc/skel/.profile
drwxr-xr-x root/root         0 2021-07-25 20:43 ./usr/
drwxr-xr-x root/root         0 2021-07-25 20:43 ./usr/bin/
-rwxr-xr-x root/root      6759 2021-07-25 20:43 ./usr/bin/bashbug
-rwxr-xr-x root/root     14648 2021-07-25 20:43 ./usr/bin/clear_console
drwxr-xr-x root/root         0 2021-07-25 20:43 ./usr/share/
drwxr-xr-x root/root         0 2021-07-25 20:43 ./usr/share/doc/
[..]
$ dpkg -I /var/cache/apt/archives/bash_5.1-3+b1_amd64.deb
 new Debian package, version 2.0.
 size 1416600 bytes: control archive=7256 bytes.
      77 bytes,     4 lines      conffiles
    1030 bytes,    27 lines      control
    4511 bytes,    64 lines      md5sums
     603 bytes,    31 lines   *  postinst             #!/bin/bash
     500 bytes,    25 lines   *  postrm               #!/bin/sh
   14536 bytes,    33 lines   *  preinst              
     289 bytes,    22 lines   *  prerm                #!/bin/bash
 Package: bash
 Source: bash (5.1-3)
 Version: 5.1-3+b1
 Architecture: amd64
 Essential: yes
 Maintainer: Matthias Klose <doko@debian.org>
 Installed-Size: 6470
 Pre-Depends: libc6 (>= 2.25), libtinfo6 (>= 6)
 Depends: base-files (>= 2.1.12), debianutils (>= 2.15)
 Recommends: bash-completion (>= 20060301-0)
 Suggests: bash-doc
 Conflicts: bash-completion (<< 20060301-0)
 Replaces: bash-completion (<< 20060301-0), bash-doc (<= 2.05-1)
 Section: shells
 Priority: required
 Multi-Arch: foreign
 Homepage: http://tiswww.case.edu/php/chet/bash/bashtop.html
 Description: GNU Bourne Again SHell
  Bash is an sh-compatible command language interpreter that executes
  commands read from the standard input or from a file.  Bash also
  incorporates useful features from the Korn and C shells (ksh and csh).
  .
  Bash is ultimately intended to be a conformant implementation of the
  IEEE POSIX Shell and Tools specification (IEEE Working Group 1003.2).
  .
  The Programmable Completion Code, by Ian Macdonald, is now found in
  the bash-completion package.
```

GOING FURTHER Comparison of versions

				Since dpkg is the program for handling Debian packages, it also provides the reference implementation of the logic of comparing version numbers. This is why it has a --compare-versions option, usable by external programs (especially configuration scripts executed by dpkg itself). This option requires three parameters: a version number, a comparison operator, and a second version number. The different possible operators are lt (strictly less than), le (less than or equal to), eq (equal), ne (not equal), ge (greater than or equal to), and gt (strictly greater than). If the comparison is correct, dpkg returns 0 (success); if not, it gives a non-zero return value (indicating failure).
			

```bash
$ dpkg --compare-versions 1.2-3 gt 1.1-4
$ echo $?
0
$ dpkg --compare-versions 1.2-3 lt 1.1-4
$ echo $?
1
$ dpkg --compare-versions 2.6.0pre3-1 lt 2.6.0-1
$ echo $?
1
```

				Note the unexpected failure of the last comparison: for dpkg, pre, usually denoting a pre-release, has no particular meaning, and this program compares the alphabetic characters in the same way as the numbers (a < b < c ...), in alphabetical order. This is why it considers â0pre3â to be greater than â0â. When we want a package's version number to indicate that it is a pre-release, we use the tilde character, â~â:
			

```bash
$ dpkg --compare-versions 2.6.0~pre3-1 lt 2.6.0-1
$ echo $?
0
```

### 5.4.4.Â dpkg's Log File

dpkg keeps a log of all of its actions in /var/log/dpkg.log. This log is extremely verbose, since it details every one of the stages through which packages handled by dpkg go. In addition to offering a way to track dpkg's behavior, it helps, above all, to keep a history of the development of the system: one can find the exact moment when each package has been installed or updated, and this information can be extremely useful in understanding a recent change in behavior. Additionally, all versions being recorded, it is easy to cross-check the information with the changelog.Debian.gz for packages in question, or even with online bug reports.
			

### 5.4.5.Â Multi-Arch Support

				All Debian packages have an Architecture field in their control information. This field can contain either âallâ (for packages that are architecture independent) or the name of the architecture that it targets (like âamd64â, âarmhfâ, â¦). In the latter case, by default, dpkg will only accept to install the package if its architecture matches the host's architecture as returned by dpkg --print-architecture.
			
				This restriction ensures that users do not end up with binaries compiled for an incorrect architecture. Everything would be perfect except that (some) computers can run binaries for multiple architectures, either natively (an âamd64â system can run âi386â binaries) or through emulators.
			

#### 5.4.5.1.Â Enabling Multi-Arch

dpkg's multi-arch support allows users to define âforeign architecturesâ that can be installed on the current system. This is simply done with dpkg --add-architecture like in the example below. There is a corresponding dpkg --remove-architecture to drop support of a foreign architecture, but it can only be used when no packages of this architecture remain.
				

```bash
# dpkg --print-architecture
amd64
# dpkg --print-foreign-architectures
# dpkg -i gcc-9-base_9.3.0-22_armhf.deb
dpkg: error processing archive gcc-9-base_9.3.0-22_armhf.deb (--install):
 package architecture (armhf) does not match system (amd64)
Errors were encountered while processing:
 gcc-9-base_9.3.0-22_armhf.deb
# dpkg --add-architecture armhf
# dpkg --add-architecture armel
# dpkg --print-foreign-architectures
armhf
armel
# dpkg -i gcc-9-base_9.3.0-22_armhf.deb
(Reading database ... 456367 files and directories currently installed.)
Preparing to unpack gcc-9-base_9.3.0-22_armhf.deb ...
Unpacking gcc-9-base:armhf (9.3.0-22) ...
Setting up gcc-9-base:armhf (9.3.0-22) ...
# dpkg --remove-architecture armhf
dpkg: error: cannot remove architecture 'armhf' currently in use by the database
# dpkg --remove-architecture armel
# dpkg --print-foreign-architectures
armhf
```

NOTE APT's multi-arch support

					APT will automatically detect when dpkg has been configured to support foreign architectures and will start downloading the corresponding Packages files during its update process.
				
					Foreign packages can then be installed with apt install package:architecture.
				

IN PRACTICE Using proprietary i386 binaries on amd64

					There are multiple use cases for multi-arch, but the most popular ones are the possibility to execute (sometimes proprietary) 32 bit binaries (i386) on 64 bit systems (amd64), and the possibility to cross-compile software for a platform or an architecture different from the host one.
				

#### 5.4.5.2.Â Multi-Arch Related Changes

					To make multi-arch actually useful and usable, libraries had to be repackaged and moved to an architecture-specific directory so that multiple copies (targeting different architectures) can be installed alongside. Such updated packages contain the âMulti-Arch: sameâ header field to tell the packaging system that the various architectures of the package can be safely co-installed (and that those packages can only satisfy dependencies of packages of the same architecture). The most important libraries have been converted since the introduction of multi-arch in Debian 7 Wheezy, but there are many libraries that will likely never be converted unless someone specifically requests it (through a bug report for example).
				

```bash
$ dpkg -s gcc-9-base
dpkg-query: error: --status needs a valid package name but 'gcc-9-base' is not: ambiguous package name 'gcc-9-base' with more than one installed instance

Use --help for help about querying packages.
$ dpkg -s gcc-9-base:amd64 gcc-9-base:armhf | grep ^Multi
Multi-Arch: same
Multi-Arch: same
$ dpkg -L libgcc-s1:amd64 |grep .so
/lib/x86_64-linux-gnu/libgcc_s.so.1
$ dpkg -S /usr/share/doc/gcc-9-base/copyright
gcc-9-base:amd64, gcc-9-base:armhf: /usr/share/doc/gcc-9-base/copyright
```

					It is worth noting that Multi-Arch: same packages must have their names qualified with their architecture to be unambiguously identifiable. They also have the possibility to share files with other instances of the same package; dpkg ensures that all packages have bit-for-bit identical files when they are shared. Last but not least, all instances of a package must have the same version. They must thus be upgraded together.
				
					Multi-Arch support also brings some interesting challenges in the way dependencies are handled. Satisfying a dependency requires either a package marked âMulti-Arch: foreignâ or a package whose architecture matches the one of the package declaring the dependency (in this dependency resolution process, architecture-independent packages are assumed to be of the same architecture than the host). A dependency can also be weakened to allow any architecture to fulfill it, with the package:any syntax, but foreign packages can only satisfy such a dependency if they are marked âMulti-Arch: allowedâ.
				[Prev5.3.Â Structure of a Source Package](sect.source-package-structure.en.md)[Up](#)[Home](index.en.md)[Next5.5.Â Coexistence with Other Packaging Systems](sect.coexistence-with-other-packaging-systems.en.md)