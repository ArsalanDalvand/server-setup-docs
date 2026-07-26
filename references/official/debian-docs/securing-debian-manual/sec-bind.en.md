---
title: "5.7.3. Chrooting the name server"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/sec-bind.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/sec-bind.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "5.7.3. Chrooting the name server"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch05s06.en.md)Securing Debian Manual[Next](ch05s08.en.md)

## 5.7.Â Securing BIND

			There are different issues that can be tackled in order to secure the Domain server daemon, which are similar to the ones considered when securing any given service: 
			
						configuring the daemon itself properly so it cannot be misused from the outside (see [SectionÂ 5.7.1, âBind configuration to avoid misuseâ](sec-bind.en.md#configure-bind)). This includes limiting possible queries from clients: zone transfers and recursive queries.
					
						limit the access of the daemon to the server itself so if it is used to break in, the damage to the system is limited. This includes running the daemon as a non-privileged user (see [SectionÂ 5.7.2, âChanging BIND's userâ](sec-bind.en.md#user-bind)) and chrooting it (see [SectionÂ 5.7.3, âChrooting the name serverâ](sec-bind.en.md#chroot-bind)).
					

### 5.7.1.Â Bind configuration to avoid misuse

				You should restrict some of the information that is served from the DNS server to outside clients so that it cannot be used to retrieve valuable information from your organization that you do not want to give away. This includes adding the following options: allow-transfer, allow-query, allow-recursion and version. You can either limit this on the global section (so it applies to all the zones served) or on a per-zone basis. This information is documented in the bind-doc package, read more on this on /usr/share/doc/bind/html/index.html once the package is installed.
			
				Imagine that your server is connected to the Internet and to your internal (your internal IP is 192.168.1.2) network (a basic multi-homed server), you do not want to give any service to the Internet and you just want to enable DNS lookups from your internal hosts. You could restrict it by including in /etc/bind/named.conf: 

```bash
options {
            allow-query { 192.168.1/24; } ;
            allow-transfer { none; } ; 
            allow-recursion { 192.168.1/24; } ;
            listen-on { 192.168.1.2; } ;
            forward { only; } ;
            forwarders { A.B.C.D; } ;
};
```

				The listen-on option makes the DNS bind to only the interface that has the internal address, but, even if this interface is the same as the interface that connects to the Internet (if you are using NAT, for example), queries will only be accepted if coming from your internal hosts. If the system has multiple interfaces and the listen-on is not present, only internal users could query, but, since the port would be accessible to outside attackers, they could try to crash (or exploit buffer overflow attacks) on the DNS server. You could even make it listen only on 127.0.0.1 if you are not giving DNS service for any other systems than yourself.
			
				The version.bind record in the chaos class contains the version of the currently running bind process. This information is often used by automated scanners and malicious individuals who wish to determine if one's bind is vulnerable to a specific attack. By providing false or no information in the version.bind record, one limits the probability that one's server will be attacked based on its published version. To provide your own version, use the version directive in the following manner: 

```bash
options { ... various options here ...
version "Not available."; };
```

				Changing the version.bind record does not provide actual protection against attacks, but it might be considered a useful safeguard.
			
				A sample named.conf configuration file might be the following: 

```bash
acl internal {
        127.0.0.1/32;           // localhost
        10.0.0.0/8;             // internal
        aa.bb.cc.dd;            // eth0 IP
};

acl friendly {
        ee.ff.gg.hh;            // slave DNS
        aa.bb.cc.dd;            // eth0 IP
        127.0.0.1/32;           // localhost
        10.0.0.0/8;             // internal
};

options {
        directory "/var/cache/bind";
        allow-query { internal; };
        allow-recursion { internal; };
        allow-transfer { none; };
};
// From here to the mysite.bogus zone 
// is basically unmodified from the debian default
logging {
        category lame-servers { null; };
        category cname { null; };   
};

zone "." {
        type hint;
        file "/etc/bind/db.root";
};

zone "localhost" {
        type master;
        file "/etc/bind/db.local";
};

zone "127.in-addr.arpa" {
        type master;
        file "/etc/bind/db.127";
};

zone "0.in-addr.arpa" {
        type master;
        file "/etc/bind/db.0";
};

zone "255.in-addr.arpa" {
        type master;
        file "/etc/bind/db.255";
};

// zones I added myself
zone "mysite.bogus" {
        type master;
        file "/etc/bind/named.mysite";
        allow-query { any; };
        allow-transfer { friendly; };
};
```

				Please (again) check the Bug Tracking System regarding Bind, specifically [http://bugs.debian.org/94760](http://bugs.debian.org/94760). Feel free to contribute to the bug report if you think you can add useful information.
			

### 5.7.2.Â Changing BIND's user

				Regarding limiting BIND's privileges you must be aware that if a non-root user runs BIND, then BIND cannot detect new interfaces automatically, for example when you put a PCMCIA card into your laptop. Check the README.Debian file in your named documentation (/usr/share/doc/bind/README.Debian) directory for more information about this issue. There have been many recent security problems concerning BIND, so switching the user is useful when possible. We will detail here the steps needed in order to do this, however, if you want to do this in an automatic way you might try the script provided in [SectionÂ B.5, âSample script to change the default Bind installation.â](bind-chuser.en.md).
			
				Notice, in any case, that this only applies to BIND version 8. In the Debian packages for BIND version 9 (since the 9.2.1-5 version, available since sarge) the bind user is created and used by setting the OPTIONS variable in /etc/default/bind9. If you are using BIND version 9 and your name server daemon is not running as the bind user verify the settings on that file.
			
				To run BIND under a different user, first create a separate user and group for it (it is not a good idea to use nobody or nogroup for every service not running as root). In this example, the user and group named will be used. You can do this by entering: 

```bash
addgroup named
adduser --system --home /home/named --no-create-home --ingroup named \
      --disabled-password --disabled-login named
```

				Notice that the user named will be quite restricted. If you want, for whatever reason, to have a less restrictive setup use: 

```bash
adduser --system --ingroup named named
```

				Now you can either edit /etc/init.d/bind with your favorite editor and change the line beginning with 

```bash
start-stop-daemon --start
```

				 to[[39]](#ftn.idm2663)

```bash
start-stop-daemon --start --quiet --exec /usr/sbin/named -- -g named -u named
```

				Or you can change (create it if it does not exit) the default configuration file (/etc/default/bind for BIND version 8) and introduce the following: 

```bash
OPTIONS="-u named -g named"
```

				 Change the permissions of files that are used by Bind, including /etc/bind/rndc.key: 

```bash
-rw-r-----    1 root     named          77 Jan  4 01:02 rndc.key
```

				 and where bind creates its pidfile, using, for example, /var/run/named instead of /var/run: 

```bash
$ mkdir /var/run/named
$ chown named.named /var/run/named
$ vi /etc/named.conf
[ ... update the configuration file to use this new location ...]
options { ...
        pid-file "/var/run/named/named.pid";
};
[ ... ]
```

				Also, in order to avoid running anything as root, change the reload line in the init.d script by substituting: 

```bash
reload)
       /usr/sbin/ndc reload
```

				to: 

```bash
reload)
        $0 stop
        sleep 1
        $0 start
```

				Note: Depending on your Debian version you might have to change the restart line too. This was fixed in Debian's bind version 1:8.3.1-2.
			
				All you need to do now is to restart bind via /etc/init.d/bind restart, and then check your syslog for two entries like this:
			

```bash
Sep  4 15:11:08 nexus named[13439]: group = named
Sep  4 15:11:08 nexus named[13439]: user = named
```

				VoilÃ ! Your named now does not run as root. If you want to read more information on why BIND does not run as non-root user on Debian systems, please check the Bug Tracking System regarding Bind, specifically [http://bugs.debian.org/50013](http://bugs.debian.org/50013) and [http://bugs.debian.org/132582](http://bugs.debian.org/132582), [http://bugs.debian.org/53550](http://bugs.debian.org/53550), [http://bugs.debian.org/52745](http://bugs.debian.org/52745), and [http://bugs.debian.org/128129](http://bugs.debian.org/128129). Feel free to contribute to the bug reports if you think you can add useful information.
			

### 5.7.3.Â Chrooting the name server

				To achieve maximum BIND security, now build a chroot jail (see [SectionÂ 5.10, âGeneral chroot and suid paranoiaâ](chroot.en.md)) around your daemon. There is an easy way to do this: the -t option (see the named(8) manual page or page 100 of [http://www.nominum.com/content/documents/bind9arm.pdf](http://www.nominum.com/content/documents/bind9arm.pdf)). This will make Bind chroot itself into the given directory without you needing to set up a chroot jail and worry about dynamic libraries. The only files that need to be in the chroot jail are: 

```bash
dev/null
etc/bind/       - should hold named.conf and all the server zones
sbin/named-xfer - if you do name transfers
var/run/named/  - should hold the PID and the name server cache (if
                  any) this directory needs to be writable by named user
var/log/named   - if you set up logging to a file, needs to be writable
                  for the named user
dev/log         - syslogd should be listening here if named is configured to
                  log through it
```

				In order for your Bind daemon to work properly it needs permission in the named files. This is an easy task since the configuration files are always at /etc/named/. Take into account that it only needs read-only access to the zone files, unless it is a secondary or cache name server. If this is your case you will have to give read-write permissions to the necessary zones (so that zone transfers from the primary server work).
			
				Also, you can find more information regarding Bind chrooting in the [http://www.tldp.org/HOWTO/Chroot-BIND-HOWTO.html](http://www.tldp.org/HOWTO/Chroot-BIND-HOWTO.html) (regarding Bind 9) and [http://www.tldp.org/HOWTO/Chroot-BIND8-HOWTO.html](http://www.tldp.org/HOWTO/Chroot-BIND8-HOWTO.html) (regarding Bind 8). This same documents should be available through the installation of the doc-linux-text (text version) or doc-linux-html (HTML version). Another useful document is [http://web.archive.org/web/20011024064030/http://www.psionic.com/papers/dns/dns-linux](http://web.archive.org/web/20011024064030/http://www.psionic.com/papers/dns/dns-linux).
			
				If you are setting up a full chroot jail (i.e. not just -t) for Bind in Debian, make sure you have the following files in it[[40]](#ftn.idm2714):  

```bash
dev/log - syslogd should be listening here
dev/null
etc/bind/named.conf 
etc/localtime
etc/group - with only a single line: "named:x:GID:"
etc/ld.so.cache - generated with ldconfig 
lib/ld-2.3.6.so
lib/libc-2.3.6.so
lib/ld-linux.so.2 - symlinked to ld-2.3.6.so
lib/libc.so.6 - symlinked to libc-2.3.6.so
sbin/ldconfig - may be deleted after setting up the chroot
sbin/named-xfer - if you do name transfers
var/run/
```

				And modify also syslogd listen on $CHROOT/dev/log so the named server can write syslog entries into the local system log.
			
				If you want to avoid problems with dynamic libraries, you can compile bind statically. You can use apt-get for this, with the source option. It can even download the packages you need to properly compile it. You would need to do something similar to: 

```bash
$ apt-get source bind
# apt-get build-dep bind
$ cd bind-8.2.5-2
  (edit src/port/linux/Makefile so CFLAGS includes the '-static'
   option)
$ dpkg-buildpackage -rfakeroot -uc -us
$ cd ..
# dpkg -i bind-8.2.5-2*deb
```

				After installation, you will need to move around the files to the chroot jail[[41]](#ftn.idm2725) you can keep the init.d scripts in /etc/init.d so that the system will automatically start the name server, but edit them to add --chroot /location_of_chroot in the calls to start-stop-daemon in those scripts or use the -t option for BIND by setting it in the OPTIONS argument at the /etc/default/bind (for version 8) or /etc/default/bind9 (for version 9) configuration file.
			
				For more information on how to set up chroots see [SectionÂ 5.10, âGeneral chroot and suid paranoiaâ](chroot.en.md).
			
				FIXME: Merge info from [http://people.debian.org/~pzn/howto/chroot-bind.sh.txt](http://people.debian.org/~pzn/howto/chroot-bind.sh.txt), [http://www.cryptio.net/~ferlatte/config/](http://www.cryptio.net/~ferlatte/config/) (Debian-specific), [http://web.archive.org/web/20021216104548/http://www.psionic.com/papers/whitep01.html](http://web.archive.org/web/20021216104548/http://www.psionic.com/papers/whitep01.html) and [http://csrc.nist.gov/fasp/FASPDocs/NISTSecuringDNS.htm](http://csrc.nist.gov/fasp/FASPDocs/NISTSecuringDNS.htm). 
			[[39]](#idm2663)
					Note that depending on your bind version you might not have the -g option, most notably if you are using bind9 in sarge (9.2.4 version).
				[[40]](#idm2714)
					This setup has not been tested for new release of Bind yet.
				[[41]](#idm2725)
					Unless you use the instdir option when calling dpkg but then the chroot jail might be a little more complex.
				[Prev5.6.Â Securing the mail service](ch05s06.en.md)[Up](#)[Home](index.en.md)[Next5.8.Â Securing Apache](ch05s08.en.md)