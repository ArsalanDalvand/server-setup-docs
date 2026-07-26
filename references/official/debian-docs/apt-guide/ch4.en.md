---
title: "4.4. Dpkg"
source: "https://www.debian.org/doc/manuals/apt-guide/ch4.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "apt-guide/ch4.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - apt-guide
aliases:
  - "4.4. Dpkg"
---
# ChapterÂ 4.Â The Interface

Table of Contents

[4.1. Startup](ch4.en.md#s4.1)

[4.2. The Status Report](ch4.en.md#s4.2)

[4.3. The Status Display](ch4.en.md#s4.3)

[4.4. Dpkg](ch4.en.md#s4.4)

Both that APT dselect method and apt-get
share the same interface. It is a simple system that generally tells you what
it will do and then goes and does it. [[2]](#ftn.id1342) After
printing out a summary of what will happen APT then will print out some
informative status messages so that you can estimate how far along it is and
how much is left to do.

## 4.1.Â Startup

Before all operations except update, APT performs a number of actions
to prepare its internal state. It also does some checks of the system's
state. At any time these operations can be performed by running
apt-get check.

```bash
# apt-get check
Reading Package Lists... Done
Building Dependency Tree... Done
```

The first thing it does is read all the package files into memory. APT uses a
caching scheme so this operation will be faster the second time it is run. If
some of the package files are not found then they will be ignored and a
warning will be printed when apt-get exits.

The final operation performs a detailed analysis of the system's
dependencies. It checks every dependency of every installed or unpacked
package and considers if it is OK. Should this find a problem then a report
will be printed out and apt-get will refuse to run.

```bash
# apt-get check
Reading Package Lists... Done
Building Dependency Tree... Done
You might want to run 'apt --fix-broken install' to correct these.
Sorry, but the following packages have unmet dependencies:
  9fonts: Depends: xlib6g but it is not installed
  uucp: Depends: mailx but it is not installed
  blast: Depends: xlib6g (>= 3.3-5) but it is not installed
  adduser: Depends: perl-base but it is not installed
  aumix: Depends: libgpmg1 but it is not installed
  debiandoc-sgml: Depends: sgml-base but it is not installed
  bash-builtins: Depends: bash (>= 2.01) but 2.0-3 is installed
  cthugha: Depends: svgalibg1 but it is not installed
           Depends: xlib6g (>= 3.3-5) but it is not installed
  libreadlineg2: Conflicts:libreadline2 (<< 2.1-2.1)
```

In this example the system has many problems, including a serious problem with
libreadlineg2. For each package that has unmet dependencies a line is printed
out indicating the package with the problem and the dependencies that are
unmet. A short explanation of why the package has a dependency problem is also
included.

There are two ways a system can get into a broken state like this. The
first is caused by dpkg missing some subtle relationships
between packages when performing upgrades. [[3]](#ftn.id1343). The second is if a package installation
fails during an operation. In this situation a package may have been
unpacked without its dependents being installed.

The second situation is much less serious than the first because APT places
certain constraints on the order that packages are installed. In both cases
supplying the -f option to apt-get
will cause APT to deduce a possible solution to the problem and then
continue on. The APT dselect method always supplies
the -f option to allow for easy continuation of failed
maintainer scripts.

However, if the -f option is used to correct a seriously
broken system caused by the first case then it is possible that it will either
fail immediately or the installation sequence will fail. In either case it is
necessary to manually use dpkg (possibly with forcing options) to correct the
situation enough to allow APT to proceed.

## 4.2.Â The Status Report

Before proceeding apt-get will present a report on what will
happen. Generally the report reflects the type of operation being performed
but there are several common elements. In all cases the lists reflect the
final state of things, taking into account the -f option
and any other relevant activities to the command being executed.

### 4.2.1.Â The Extra Package list

```bash
The following extra packages will be installed:
  libdbd-mysql-perl xlib6 zlib1 xzx libreadline2 libdbd-msql-perl
  mailpgp xdpkg fileutils pinepgp zlib1g xlib6g perl-base
  bin86 libgdbm1 libgdbmg1 quake-lib gmp2 bcc xbuffy
  squake pgp-i python-base debmake ldso perl libreadlineg2
  ssh
```

The Extra Package list shows all of the packages that will be installed or
upgraded in excess of the ones mentioned on the command line. It is only
generated for an install command. The listed packages are
often the result of an Auto Install.

### 4.2.2.Â The Packages to Remove

```bash
The following packages will be REMOVED:
  xlib6-dev xpat2 tk40-dev xkeycaps xbattle xonix
  xdaliclock tk40 tk41 xforms0.86 ghostview xloadimage xcolorsel
  xadmin xboard perl-debug tkined xtetris libreadline2-dev perl-suid
  nas xpilot xfig
```

The Packages to Remove list shows all of the packages that will be removed
from the system. It can be shown for any of the operations and should be given
a careful inspection to ensure nothing important is to be taken off. The
-f option is especially good at generating packages to
remove so extreme care should be used in that case. The list may contain
packages that are going to be removed because they are only partially
installed, possibly due to an aborted installation.

### 4.2.3.Â The New Packages list

```bash
The following NEW packages will installed:
  zlib1g xlib6g perl-base libgdbmg1 quake-lib gmp2 pgp-i python-base
```

The New Packages list is simply a reminder of what will happen. The packages
listed are not presently installed in the system but will be when APT is done.

### 4.2.4.Â The Kept Back list

```bash
The following packages have been kept back
  compface man-db tetex-base msql libpaper svgalib1
  gs snmp arena lynx xpat2 groff xscreensaver
```

Whenever the whole system is being upgraded there is the possibility that new
versions of packages cannot be installed because they require new things or
conflict with already installed things. In this case the package will appear
in the Kept Back list. The best way to convince packages listed there to
install is with apt-get install or by using
dselect to resolve their problems.

### 4.2.5.Â Held Packages warning

```bash
The following held packages will be changed:
  cvs
```

Sometimes you can ask APT to install a package that is on hold, in such a case
it prints out a warning that the held package is going to be changed. This
should only happen during dist-upgrade or install.

### 4.2.6.Â Final summary

Finally, APT will print out a summary of all the changes that will occur.

```bash
206 packages upgraded, 8 newly installed, 23 to remove and 51 not upgraded.
12 packages not fully installed or removed.
Need to get 65.7M/66.7M of archives. After unpacking 26.5M will be used.
```

The first line of the summary simply is a reduced version of all of the lists
and includes the number of upgrades - that is packages already installed that
have new versions available. The second line indicates the number of poorly
configured packages, possibly the result of an aborted installation. The final
line shows the space requirements that the installation needs. The first pair
of numbers refer to the size of the archive files. The first number indicates
the number of bytes that must be fetched from remote locations and the second
indicates the total size of all the archives required. The next number
indicates the size difference between the presently installed packages and the
newly installed packages. It is roughly equivalent to the space required in
/usr after everything is done. If a large number of packages are being removed
then the value may indicate the amount of space that will be freed.

Some other reports can be generated by using the -u option to show packages to
upgrade, they are similar to the previous examples.

## 4.3.Â The Status Display

During the download of archives and package files APT prints out a series of
status messages.

```bash
# apt-get update
Get:1 http://ftp.de.debian.org/debian/ stable/main Packages
Get:2 http://llug.sep.bnl.gov/debian/ testing/contrib Packages
Hit http://llug.sep.bnl.gov/debian/ testing/main Packages
Get:4 http://ftp.de.debian.org/debian/ unstable/binary-i386/ Packages
Get:5 http://llug.sep.bnl.gov/debian/ testing/non-free Packages
11% [5 testing/non-free `Waiting for file' 0/32.1k 0%] 2203b/s 1m52s
```

The lines starting with Get are printed out when APT
begins to fetch a file while the last line indicates the progress of the
download. The first percent value on the progress line indicates the total
percent done of all files. Unfortunately since the size of the Package files
is unknown apt-get update estimates the percent done which
causes some inaccuracies.

The next section of the status line is repeated once for each download
thread and indicates the operation being performed and some useful
information about what is happening. Sometimes this section will simply
read Forking which means the OS is loading the download
module. The first word after the [ is the fetch number as shown on the
history lines. The next word is the short form name of the object being
downloaded. For archives it will contain the name of the package that is
being fetched.

Inside of the single quote is an informative string indicating the progress of
the negotiation phase of the download. Typically it progresses from
Connecting to Waiting for file to
Downloading or Resuming. The final
value is the number of bytes downloaded from the remote site. Once the
download begins this is represented as 102/10.2k indicating
that 102 bytes have been fetched and 10.2 kilobytes is expected. The total
size is always shown in 4 figure notation to preserve space. After the size
display is a percent meter for the file itself. The second last element is the
instantaneous average speed. This values is updated every 5 seconds and
reflects the rate of data transfer for that period. Finally is shown the
estimated transfer time. This is updated regularly and reflects the time to
complete everything at the shown transfer rate.

The status display updates every half second to provide a constant feedback on
the download progress while the Get lines scroll back whenever a new file is
started. Since the status display is constantly updated it is unsuitable for
logging to a file, use the -q option to remove the status
display.

## 4.4.Â Dpkg

APT uses dpkg for installing the archives and will
switch over to the dpkg interface once downloading is
completed. dpkg will also ask a number of questions as
it processes the packages and the packages themselves may also ask several
questions. Before each question there is usually a description of what it
is asking and the questions are too varied to discuss completely here.

[[2]](#id1342) The
dselect method actually is a set of wrapper scripts to
apt-get. The method actually provides more functionality
than is present in apt-get alone.

[[3]](#id1343) APT however
considers all known dependencies and attempts to prevent broken
packages