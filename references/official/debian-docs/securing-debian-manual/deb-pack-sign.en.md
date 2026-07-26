---
title: "7.5.5. Alternative per-package signing scheme"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/deb-pack-sign.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/deb-pack-sign.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "7.5.5. Alternative per-package signing scheme"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch07s04.en.md)Securing Debian Manual[Next](sec-tools.en.md)

## 7.5.Â Package signing in Debian

			This section could also be titled "how to upgrade/update safely your Debian GNU/Linux system" and it deserves its own section basically because it is an important part of the Security Infrastructure. Package signing is an important issue since it avoids tampering of packages distributed in mirrors and of downloads with man-in-the-middle attacks. Automatic software update is an important feature but it's also important to remove security threats that could help the distribution of trojans and the compromise of systems during updates [[47]](#ftn.idm3301)

			FIXME: probably the Internet Explorer vulnerability handling. certificate chains has an impact on security updates on Microsoft Windows.
		
			Debian does not provide signed packages but provides a mechanism available since Debian 4.0 (codename etch) to check for downloaded package's integrity[[48]](#ftn.idm3307). For more information, see [SectionÂ 7.5.2, âSecure aptâ](deb-pack-sign.en.md#apt-0.6).
		
			This issue is better described in the [http://www.cryptnet.net/fdp/crypto/strong_distro.html](http://www.cryptnet.net/fdp/crypto/strong_distro.html) by V. Alex Brennen.
		

### 7.5.1.Â The current scheme for package signature checks

				The current scheme for package signature checking using apt is: 
				
							the Release file includes the MD5 sum of Packages.gz (which contains the MD5 sums of packages) and will be signed. The signature is one of a trusted source.
						
							This signed Release file is downloaded by 'apt-get update' and stored along with Packages.gz.
						
							When a package is going to be installed, it is first downloaded, then the MD5 sum is generated.
						
							The signed Release file is checked (signature ok) and it extracts from it the MD5 sum for the Packages.gz file, the Packages.gz checksum is generated and (if ok) the MD5 sum of the downloaded package is extracted from it.
						
							If the MD5 sum from the downloaded package is the same as the one in the Packages.gz file the package will be installed, otherwise the administrator will be alerted and the package will be left in the cache (so the administrator can decide whether to install it or not). If the package is not in the Packages.gz and the administrator has configured the system to only install checked packages it will not be installed either.
						

				By following the chain of MD5 sums apt is capable of verifying that a package originates from a a specific release. This is less flexible than signing each package one by one, but can be combined with that scheme too (see below).
			
				This scheme is [http://lists.debian.org/debian-devel/2003/12/msg01986.html](http://lists.debian.org/debian-devel/2003/12/msg01986.html ) in apt 0.6 and is available since the Debian 4.0 release. For more information see [SectionÂ 7.5.2, âSecure aptâ](deb-pack-sign.en.md#apt-0.6). Packages that provide a front-end to apt need to be modified to adapt to this new feature; this is the case of aptitude which was [http://lists.debian.org/debian-devel/2005/03/msg02641.html](http://lists.debian.org/debian-devel/2005/03/msg02641.html) to adapt to this scheme. Front-ends currently known to work properly with this feature include aptitude and synaptic.
			
				Package signing has been discussed in Debian for quite some time, for more information you can read: [http://www.debian.org/News/weekly/2001/8/](http://www.debian.org/News/weekly/2001/8/) and [http://www.debian.org/News/weekly/2000/11/](http://www.debian.org/News/weekly/2000/11/).
			

### 7.5.2.Â Secure apt

				The apt 0.6 release, available since Debian 4.0 etch and later releases, includes apt-secure (also known as secure apt) which is a tool that will allow a system administrator to test the integrity of the packages downloaded through the above scheme. This release includes the tool apt-key for adding new keys to apt's keyring, which by default includes only the current Debian archive signing key.
			
				These changes are based on the patch for apt (available in [http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=203741](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=203741)) which provides this implementation.
			
				Secure apt works by checking the distribution through the Release file, as discussed in [SectionÂ 7.5.3, âPer distribution release checkâ](deb-pack-sign.en.md#check-releases). Typically, this process will be transparent to the administrator although you will need to intervene every year[[49]](#ftn.idm3362) to add the new archive key when it is rotated, for more information on the steps an administrator needs to take a look at [SectionÂ 7.5.3.7, âSafely adding a keyâ](deb-pack-sign.en.md#secure-apt-add-key).
			
				This feature is still under development, if you believe you find bugs in it, please, make first sure you are using the latest version (as this package might change quite a bit before it is finally released) and, if running the latest version, submit a bug against the apt package.
			
				You can find more information at [http://wiki.debian.org/SecureApt](http://wiki.debian.org/SecureApt) and the official documentation: [http://www.enyo.de/fw/software/apt-secure/](http://www.enyo.de/fw/software/apt-secure/) and [https://web.archive.org/web/20070206063141/http://www.syntaxpolice.org/apt-secure/](https://web.archive.org/web/20070206063141/http://www.syntaxpolice.org/apt-secure/).
			

### 7.5.3.Â Per distribution release check

				This section describes how the distribution release check mechanism works, it was written by Joey Hess and is also available at the [http://wiki.debian.org/SecureApt](http://wiki.debian.org/SecureApt).
			

#### 7.5.3.1.Â Basic concepts

					Here are a few basic concepts that you'll need to understand for the rest of this section.
				
					A checksum is a method of taking a file and boiling it down to a reasonably short number that uniquely identifies the content of the file. This is a lot harder to do well than it might seem, and the most commonly used type of checksum, the MD5 sum, is in the process of being broken.
				
					Public key cryptography is based on pairs of keys, a public key and a private key. The public key is given out to the world; the private key must be kept a secret. Anyone possessing the public key can encrypt a message so that it can only be read by someone possessing the private key. It's also possible to use a private key to sign a file, not encrypt it. If a private key is used to sign a file, then anyone who has the public key can check that the file was signed by that key. No one who doesn't have the private key can forge such a signature.
				
					These keys are quite long numbers (1024 to 2048 digits or longer), and to make them easier to work with they have a key id, which is a shorter, 8 or 16 digit number that can be used to refer to them.
				
gpg is the tool used in secure apt to sign files and check their signatures.
				
apt-key is a program that is used to manage a keyring of gpg keys for secure apt. The keyring is kept in the file /etc/apt/trusted.gpg (not to be confused with the related but not very interesting /etc/apt/trustdb.gpg). apt-key can be used to show the keys in the keyring, and to add or remove a key.
				

#### 7.5.3.2.Â Release checksums

					A Debian archive contains a Release file, which is updated each time any of the packages in the archive change. Among other things, the Release file contains some MD5 sums of other files in the archive. An excerpt of an example Release file: 

```bash
MD5Sum:
 6b05b392f792ba5a436d590c129de21f            3453 Packages
 1356479a23edda7a69f24eb8d6f4a14b            1131 Packages.gz
 2a5167881adc9ad1a8864f281b1eb959            1715 Sources
 88de3533bf6e054d1799f8e49b6aed8b             658 Sources.gz
```

					The Release files also include SHA-1 checksums, which will be useful once MD5 sums become fully broken, however apt doesn't use them yet.
				
					Now if we look inside a Packages file, we'll find more MD5 sums, one for each package listed in it. For example: 

```bash
Package: uqm
    Priority: optional
    ...
    Filename: unstable/uqm_0.4.0-1_i386.deb
    Size: 580558
    MD5sum: 864ec6157c1eea88acfef44d0f34d219
```

					These two checksums can be used to verify that you have downloaded a correct copy of the Packages file, with a md5sum that matches the one in the Release file. And when it downloads an individual package, it can also check its md5sum against the content of the Packages file. If apt fails at either of these steps, it will abort.
				
					None of this is new in secure apt, but it does provide the foundation. Notice that so far there is one file that apt doesn't have a way to check: The Release file. Secure apt is all about making apt verify the Release file before it does anything else with it, and plugging this hole, so that there is a chain of verification from the package that you are going to install all the way back to the provider of the package.
				

#### 7.5.3.3.Â Verification of the Release file

					To verify the Release file, a gpg signature is added for the Release file. This is put in a file named Release.gpg that is shipped alongside the Release file. It looks something like this [[50]](#ftn.idm3415) , although only gpg actually looks at its contents normally: 

```bash
-----BEGIN PGP SIGNATURE-----
Version: GnuPG v1.4.1 (GNU/Linux)

iD8DBQBCqKO1nukh8wJbxY8RAsfHAJ9hu8oGNRAl2MSmP5+z2RZb6FJ8kACfWvEx
UBGPVc7jbHHsg78EhMBlV/U=
=x6og
-----END PGP SIGNATURE-----
```

#### 7.5.3.4.Â Check of Release.gpg by apt

					Secure apt always downloads Release.gpg files when it's downloading Release files, and if it cannot download the Release.gpg, or if the signature is bad, it will complain, and will make note that the Packages files that the Release file points to, and all the packages listed therein, are from an untrusted source. Here's how it looks during an apt-get update: 

```bash
W: GPG error: http://ftp.us.debian.org testing Release: The following signatures
 couldn't be verified because the public key is not available: NO_PUBKEY 010908312D230C5F
```

					Note that the second half of the long number is the key id of the key that apt doesn't know about, in this case that's 2D230C5F.
				
					If you ignore that warning and try to install a package later, apt will warn again: 

```bash
WARNING: The following packages cannot be authenticated!
  libglib-perl libgtk2-perl
Install these packages without verification [y/N]?
```

					If you say Y here you have no way to know if the file you're getting is the package you're supposed to install, or if it's something else entirely that somebody that can intercept the communication against the server[[51]](#ftn.idm3434) has arranged for you, containing a nasty suprise.
				
					Note that you can disable these checks by running apt with --allow-unauthenticated.
				
					It's also worth noting that newer versions of the Debian installer use the same signed Release file mechanism during their debootstrap of the Debian base system, before apt is available, and that the installer even uses this system to verify pieces of itself that it downloads from the net. Also, Debian does not currently sign the Release files on its CDs; apt can be configured to always trust packages from CDs so this is not a large problem.
				

#### 7.5.3.5.Â How to tell apt what to trust

					So the security of the whole system depends on there being a Release.gpg file, which signs a Release file, and of apt checking that signature using gpg. To check the signature, it has to know the public key of the person who signed the file. These keys are kept in apt's own keyring (/etc/apt/trusted.gpg), and managing the keys is where secure apt comes in.
				
					By default, Debian systems come preconfigured with the Debian archive key in the keyring. 

```bash
# apt-key list
/etc/apt/trusted.gpg
--------------------
pub   1024D/4F368D5D 2005-01-31 [expires: 2006-01-31]
uid                  Debian Archive Automatic Signing Key (2005) <ftpmaster@debian.org>
```

					Here 4F368D5D is the key id, and notice that this key was only valid for a one year period. Debian rotates these keys as a last line of defense against some sort of security breach breaking a key.
				
					That will make apt trust the official Debian archive, but if you add some other apt repository to /etc/apt/sources.list, you'll also have to give apt its key if you want apt to trust it. Once you have the key and have verified it, it's a simple matter of running apt-key add file to add it. Getting the key and verifying it are the trickier parts.
				

#### 7.5.3.6.Â Finding the key for a repository

					The debian-archive-keyring package is used to distribute keys to apt. Upgrades to this package can add (or remove) gpg keys for the main Debian archive.
				
					For other archives, there is not yet a standard location where you can find the key for a given apt repository. There's a rough standard of putting the key up on the web page for the repository or as a file in the repository itself, but no real standard, so you might have to hunt for it.
				
					The Debian archive signing key is available at [https://ftp-master.debian.org/keys.html](https://ftp-master.debian.org/keys.html).[[52]](#ftn.idm3463)

gpg itself has a standard way to distribute keys, using a keyserver that gpg can download a key from and add it to its keyring. For example: 

```bash
$ gpg --keyserver pgpkeys.mit.edu --recv-key 2D230C5F
gpg: requesting key 2D230C5F from hkp server pgpkeys.mit.edu
gpg: key 2D230C5F: public key "Debian Archive Automatic Signing Key (2006) <ftpm
aster@debian.org>" imported
gpg: Total number processed: 1
gpg:               imported: 1
```

					You can then export that key from your own keyring and feed it to apt-key: 

```bash
$ gpg -a --export 2D230C5F | sudo apt-key add -
gpg: no ultimately trusted keys found
OK
```

					The "gpg: no ultimately trusted keys found" warning means that gpg was not configured to ultimately trust a specific key. Trust settings are part of OpenPGPs Web-of-Trust which does not apply here. So there is no problem with this warning. In typical setups the user's own key is ultimately trusted.
				

#### 7.5.3.7.Â Safely adding a key

					By adding a key to apt's keyring, you're telling apt to trust everything signed by the key, and this lets you know for sure that apt won't install anything not signed by the person who possesses the private key. But if you're sufficiently paranoid, you can see that this just pushes things up a level, now instead of having to worry if a package, or a Release file is valid, you can worry about whether you've actually gotten the right key. Is the key file from [https://ftp-master.debian.org/keys.html](https://ftp-master.debian.org/keys.html) mentioned above really Debian's archive signing key, or has it been modified (or this document lies).
				
					It's good to be paranoid in security, but verifying things from here is harder. gpg has the concept of a chain of trust, which can start at someone you're sure of, who signs someone's key, who signs some other key, etc., until you get to the archive key. If you're sufficiently paranoid you'll want to check that your archive key is signed by a key that you can trust, with a trust chain that goes back to someone you know personally. If you want to do this, visit a Debian conference or perhaps a local LUG for a key signing [[53]](#ftn.idm3480).
				
					If you can't afford this level of paranoia, do whatever feels appropriate to you when adding a new apt source and a new key. Maybe you'll want to mail the person providing the key and verify it, or maybe you're willing to take your chances with downloading it and assuming you got the real thing. The important thing is that by reducing the problem to what archive keys to trust, secure apt lets you be as careful and secure as it suits you to be.
				

#### 7.5.3.8.Â Verifying key integrity

					You can verify the fingerprint as well as the signatures on the key. Retrieving the fingerprint can be done for multiple sources, you can talk to Debian Developers on IRC, read the mailing list where the key change will be announced or any other additional means to verify the fingerprint. For example you can do this: 

```bash
$ GET http://ftp-master.debian.org/ziyi_key_2006.asc | gpg --import
gpg: key 2D230C5F: public key "Debian Archive Automatic Signing Key (2006)
  <ftpmaster&debian.org>" imported
gpg: Total number processed: 1
gpg:               imported: 1
$ gpg --check-sigs --fingerprint 2D230C5F
pub   1024D/2D230C5F 2006-01-03 [expires: 2007-02-07]
      Key fingerprint = 0847 50FC 01A6 D388 A643  D869 0109 0831 2D23 0C5F
uid   Debian Archive Automatic Signing Key (2006) <ftpmaster@debian.org>
sig!3        2D230C5F 2006-01-03  Debian Archive Automatic Signing Key
                                  (2006) <ftpmaster@debian.org>
sig!         2A4E3EAA 2006-01-03  Anthony Towns <aj@azure.humbug.org.au>
sig!         4F368D5D 2006-01-03  Debian Archive Automatic Signing Key
                                  (2005) <ftpmaster@debian.org>
sig!         29982E5A 2006-01-04  Steve Langasek <vorlon@dodds.net>
sig!         FD6645AB 2006-01-04  Ryan Murray <rmurray@cyberhqz.com>
sig!         AB2A91F5 2006-01-04  James Troup <james@nocrew.org>
```

					 and then as in [SectionÂ 7.5, âPackage signing in Debianâ](deb-pack-sign.en.md) check the trust path from your key (or a key you trust) to at least one of the keys used to sign the archive key. If you are sufficiently paranoid you will tell apt to trust the key only if you find an acceptable path: 

```bash
$ gpg --export -a 2D230C5F | sudo apt-key add -
Ok
```

					Note that the key is signed with the previous archive key, so theoretically you can just build on your previous trust.
				

#### 7.5.3.9.Â Debian archive key yearly rotation

					As mentioned above, the Debian archive signing key is changed each year, in January. Since secure apt is young, we don't have a great deal of experience with changing the key and there are still rough spots.
				
					In January 2006, a new key for 2006 was made and the Release file began to be signed by it, but to try to avoid breaking systems that had the old 2005 key, the Release file was signed by that as well. The intent was that apt would accept one signature or the other depending on the key it had, but apt turned out to be buggy and refused to trust the file unless it had both keys and was able to check both signatures. This was fixed in apt version 0.6.43.1. There was also confusion about how the key was distributed to users who already had systems using secure apt; initially it was uploaded to the web site with no announcement and no real way to verify it and users were forced to download it by hand.
				
					In January 2006, a new key for 2006 was made and the Release file began to be signed by it, but to try to avoid breaking systems that had the old 2005 key, the Release file was signed by that as well. In order to prevent confusion on the best distribution mechanism for users who already have systems using secure apt, the debian-archive-keyring package was introduced, which manages apt keyring updates. 
				

#### 7.5.3.10.Â Known release checking problems

					One not so obvious problem is that if your clock is very far off, secure apt will not work. If it's set to a date in the past, such as 1999, apt will fail with an unhelpful message such as this: 

```bash
W: GPG error: http://archive.progeny.com sid Release: Unknown error executing gpg
```

					Although apt-key list will make the problem plain: 

```bash
gpg: key 2D230C5F was created 192324901 seconds in the future (time warp or clock problem)
gpg: key 2D230C5F was created 192324901 seconds in the future (time warp or clock problem)
pub   1024D/2D230C5F 2006-01-03
uid                  Debian Archive Automatic Signing Key (2006) <ftpmaster@debian.org>
```

					If it's set to a date too far in the future, apt will treat the keys as expired.
				
					Another problem you may encouter if using testing or unstable is that if you have not run apt-get update lately and apt-get install a package, apt might complain that it cannot be authenticated (why does it do this?). apt-get update will fix this.
				

#### 7.5.3.11.Â Manual per distribution release check

					In case you want to add now the additional security checks and don't want or cannot run the latest apt version[[54]](#ftn.idm3514) you can use the script below, provided by Anthony Towns. This script can automatically do some new security checks to allow the user to be sure that the software s/he's downloading matches the software Debian's distributing. This stops Debian developers from hacking into someone's system without the accountability provided by uploading to the main archive, or mirrors mirroring something almost, but not quite like Debian, or mirrors providing out of date copies of unstable with known security problems.
				
					This sample code, renamed as apt-check-sigs, should be used in the following way: 

```bash
# apt-get update
# apt-check-sigs
(...results...)
# apt-get dist-upgrade
```

					First you need to: 
					
								get the keys the archive software uses to sign Release files from [https://ftp-master.debian.org/keys.html](https://ftp-master.debian.org/keys.html) and add them to ~/.gnupg/trustedkeys.gpg (which is what gpgv uses by default). 

```bash
gpg --no-default-keyring --keyring trustedkeys.gpg --import ziyi_key_2006.asc
```

								remove any /etc/apt/sources.list lines that don't use the normal "dists" structure, or change the script so that it works with them.
							
								be prepared to ignore the fact that Debian security updates don't have signed Release files, and that Sources files don't have appropriate checksums in the Release file (yet).
							
								be prepared to check that the appropriate sources are signed by the appropriate keys.
							

					This is the example code for apt-check-sigs, the latest version can be retrieved from [http://people.debian.org/~ajt/apt-check-sigs](http://people.debian.org/~ajt/apt-check-sigs). This code is currently in beta, for more information read [http://lists.debian.org/debian-devel/2002/07/msg00421.html](http://lists.debian.org/debian-devel/2002/07/msg00421.html). 

```bash
#!/bin/bash

# Copyright (c) 2001 Anthony Towns <ajt@debian.org>
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

rm -rf /tmp/apt-release-check
mkdir /tmp/apt-release-check || exit 1
cd /tmp/apt-release-check

>OK
>MISSING
>NOCHECK
>BAD

arch=`dpkg --print-installation-architecture`

am_root () {
        [ `id -u` -eq 0 ]
}

get_md5sumsize () {
        cat "$1" | awk '/^MD5Sum:/,/^SHA1:/' | 
          MYARG="$2" perl -ne '@f = split /\s+/; if ($f[3] eq $ENV{"MYARG"}) {
print "$f[1] $f[2]\n"; exit(0); }'
}

checkit () {
        local FILE="$1"
        local LOOKUP="$2"

        Y="`get_md5sumsize Release "$LOOKUP"`"
        Y="`echo "$Y" | sed 's/^ *//;s/  */ /g'`"

        if [ ! -e "/var/lib/apt/lists/$FILE" ]; then
                if [ "$Y" = "" ]; then
                        # No file, but not needed anyway
                        echo "OK"
                        return
                fi
                echo "$FILE" >>MISSING
                echo "MISSING $Y"
                return
        fi
        if [ "$Y" = "" ]; then
                echo "$FILE" >>NOCHECK
                echo "NOCHECK"
                return
        fi
        X="`md5sum < /var/lib/apt/lists/$FILE | cut -d\  -f1` `wc -c < /var/lib
/apt/lists/$FILE`"
        X="`echo "$X" | sed 's/^ *//;s/  */ /g'`"
        if [ "$X" != "$Y" ]; then
                echo "$FILE" >>BAD
                echo "BAD"
                return
        fi
        echo "$FILE" >>OK
        echo "OK"
}

echo
echo "Checking sources in /etc/apt/sources.list:"
echo "~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~"
echo
(echo "You should take care to ensure that the distributions you're downloading
"
echo "are the ones you think you are downloading, and that they are as up to"
echo "date as you would expect (testing and unstable should be no more than"
echo "two or three days out of date, stable-updates no more than a few weeks"
echo "or a month)."
) | fmt
echo

cat /etc/apt/sources.list | 
  sed 's/^ *//' | grep '^[^#]' |
  while read ty url dist comps; do
        if [ "${url%%:*}" = "http" -o "${url%%:*}" = "ftp" ]; then
                baseurl="${url#*://}"
        else
                continue
        fi

        echo "Source: ${ty} ${url} ${dist} ${comps}"

        rm -f Release Release.gpg
        lynx -reload -dump "${url}/dists/${dist}/Release" >/dev/null 2>&1
        wget -q -O Release "${url}/dists/${dist}/Release"

        if ! grep -q '^' Release; then
                echo "  * NO TOP-LEVEL Release FILE"
                >Release
        else
                origline=`sed -n 's/^Origin: *//p' Release | head -1`
                lablline=`sed -n 's/^Label: *//p' Release | head -1`
                suitline=`sed -n 's/^Suite: *//p' Release | head -1`
                codeline=`sed -n 's/^Codename: *//p' Release | head -1`
                dateline=`grep "^Date:" Release | head -1`
                dscrline=`grep "^Description:" Release | head -1`
                echo "  o Origin: $origline/$lablline"
                echo "  o Suite: $suitline/$codeline"
                echo "  o $dateline"
                echo "  o $dscrline"

                if [ "${dist%%/*}" != "$suitline" -a "${dist%%/*}" != "$codeline" ]; then
                        echo "  * WARNING: asked for $dist, got $suitline/$codeline"
                fi

                lynx -reload -dump "${url}/dists/${dist}/Release.gpg" >/dev/null 2>&1
                wget -q -O Release.gpg "${url}/dists/${dist}/Release.gpg"

                gpgv --status-fd 3 Release.gpg Release 3>&1 >/dev/null 2>&1 | sed -n "s/^\[GNUPG:\] //p" | (okay=0; err=""; while read gpgcode rest; do
                        if [ "$gpgcode" = "GOODSIG" ]; then
                            if [ "$err" != "" ]; then
                                echo "  * Signed by ${err# } key: ${rest#* }"
                            else
                                echo "  o Signed by: ${rest#* }"
                                okay=1
                            fi
                            err=""
                        elif [ "$gpgcode" = "BADSIG" ]; then
                            echo "  * BAD SIGNATURE BY: ${rest#* }"
                            err=""
                        elif [ "$gpgcode" = "ERRSIG" ]; then
                            echo "  * COULDN'T CHECK SIGNATURE BY KEYID: ${rest %% *}"
                            err=""
                        elif [ "$gpgcode" = "SIGREVOKED" ]; then
                            err="$err REVOKED"
                        elif [ "$gpgcode" = "SIGEXPIRED" ]; then
                            err="$err EXPIRED"
                        fi
                    done
                    if [ "$okay" != 1 ]; then
                        echo "  * NO VALID SIGNATURE"
                        >Release
                    fi)
        fi
        okaycomps=""
        for comp in $comps; do
                if [ "$ty" = "deb" ]; then
                        X=$(checkit "`echo "${baseurl}/dists/${dist}/${comp}/binary-${arch}/Release" | sed 's,//*,_,g'`" "${comp}/binary-${arch}/Release")
                        Y=$(checkit "`echo "${baseurl}/dists/${dist}/${comp}/binary-${arch}/Packages" | sed 's,//*,_,g'`" "${comp}/binary-${arch}/Packages")
                        if [ "$X $Y" = "OK OK" ]; then
                                okaycomps="$okaycomps $comp"
                        else
                                echo "  * PROBLEMS WITH $comp ($X, $Y)"
                        fi
                elif [ "$ty" = "deb-src" ]; then
                        X=$(checkit "`echo "${baseurl}/dists/${dist}/${comp}/source/Release" | sed 's,//*,_,g'`" "${comp}/source/Release")
                        Y=$(checkit "`echo "${baseurl}/dists/${dist}/${comp}/source/Sources" | sed 's,//*,_,g'`" "${comp}/source/Sources")
                        if [ "$X $Y" = "OK OK" ]; then
                                okaycomps="$okaycomps $comp"
                        else
                                echo "  * PROBLEMS WITH component $comp ($X, $Y)"
                        fi
                fi
        done
        [ "$okaycomps" = "" ] || echo "  o Okay:$okaycomps"
        echo
  done

echo "Results"
echo "~~~~~~~"
echo

allokay=true

cd /tmp/apt-release-check
diff <(cat BAD MISSING NOCHECK OK | sort) <(cd /var/lib/apt/lists && find . -type f -maxdepth 1 | sed 's,^\./,,g' | grep '_' | sort) | sed -n 's/^> //p' >UNVALIDATED

cd /tmp/apt-release-check
if grep -q ^ UNVALIDATED; then
    allokay=false
    (echo "The following files in /var/lib/apt/lists have not been validated."
    echo "This could turn out to be a harmless indication that this script"
    echo "is buggy or out of date, or it could let trojaned packages get onto"
    echo "your system."
    ) | fmt
    echo
    sed 's/^/    /' < UNVALIDATED
    echo
fi

if grep -q ^ BAD; then
    allokay=false
    (echo "The contents of the following files in /var/lib/apt/lists does not"
    echo "match what was expected. This may mean these sources are out of date,"
    echo "that the archive is having problems, or that someone is actively"
    echo "using your mirror to distribute trojans."
    if am_root; then 
        echo "The files have been renamed to have the extension .FAILED and"
        echo "will be ignored by apt."
        cat BAD | while read a; do
            mv /var/lib/apt/lists/$a /var/lib/apt/lists/${a}.FAILED
        done
    fi) | fmt
    echo
    sed 's/^/    /' < BAD
    echo
fi

if grep -q ^ MISSING; then
    allokay=false
    (echo "The following files from /var/lib/apt/lists were missing. This"
    echo "may cause you to miss out on updates to some vulnerable packages."
    ) | fmt
    echo
    sed 's/^/    /' > MISSING
    echo
fi

if grep -q ^ NOCHECK; then
    allokay=false
    (echo "The contents of the following files in /var/lib/apt/lists could not"
    echo "be validated due to the lack of a signed Release file, or the lack"
    echo "of an appropriate entry in a signed Release file. This probably"
    echo "means that the maintainers of these sources are slack, but may mean"
    echo "these sources are being actively used to distribute trojans."
    if am_root; then 
        echo "The files have been renamed to have the extension .FAILED and"
        echo "will be ignored by apt."
        cat NOCHECK | while read a; do
            mv /var/lib/apt/lists/$a /var/lib/apt/lists/${a}.FAILED
        done
    fi) | fmt
    echo
    sed 's/^/    /' > NOCHECK
    echo
fi

if $allokay; then
    echo 'Everything seems okay!'
    echo
fi

rm -rf /tmp/apt-release-check
```

					You might need to apply the following patch for sid since md5sum adds an '-' after the sum when the input is stdin: 

```bash
@@ -37,7 +37,7 @@
        local LOOKUP="$2"

        Y="`get_md5sumsize Release "$LOOKUP"`"
-       Y="`echo "$Y" | sed 's/^ *//;s/  */ /g'`"
+       Y="`echo "$Y" | sed 's/-//;s/^ *//;s/  */ /g'`"

        if [ ! -e "/var/lib/apt/lists/$FILE" ]; then
                if [ "$Y" = "" ]; then
@@ -55,7 +55,7 @@
                return
        fi
        X="`md5sum < /var/lib/apt/lists/$FILE` `wc -c < /var/lib/apt/lists/$FILE`"
-       X="`echo "$X" | sed 's/^ *//;s/  */ /g'`"
+       X="`echo "$X" | sed 's/-//;s/^ *//;s/  */ /g'`"
        if [ "$X" != "$Y" ]; then
                echo "$FILE" >>BAD
                echo "BAD"
```

### 7.5.4.Â Release check of non Debian sources

				Notice that, when using the latest apt version (with secure apt) no extra effort should be required on your part unless you use non-Debian sources, in which case an extra confirmation step will be required by apt-get. This is avoided by providing Release and Release.gpg files in the non-Debian sources. The Release file can be generated with apt-ftparchive (available in apt-utils 0.5.0 and later), the Release.gpg is just a detached signature. To generate both follow this simple procedure: 

```bash
$ rm -f dists/unstable/Release
$ apt-ftparchive release dists/unstable > dists/unstable/Release
$ gpg --sign -ba -o dists/unstable/Release.gpg dists/unstable/Release
```

### 7.5.5.Â Alternative per-package signing scheme

				The additional scheme of signing each and every packages allows packages to be checked when they are no longer referenced by an existing Packages file, and also third-party packages where no Packages ever existed for them can be also used in Debian but will not be default scheme.
			
				This package signing scheme can be implemented using debsig-verify and debsigs. These two packages can sign and verify embedded signatures in the .deb itself. Debian already has the capability to do this now, but there is no feature plan to implement the policy or other tools since the archive signing scheme is prefered. These tools are available for users and archive administrators that would rather use this scheme instead.
			
				Latest dpkg versions (since 1.9.21) incorporate a [http://lists.debian.org/debian-dpkg/2001/03/msg00024.html](http://lists.debian.org/debian-dpkg/2001/03/msg00024.html) that provides this functionality as soon as debsig-verify is installed.
			
				NOTE: Currently /etc/dpkg/dpkg.cfg ships with "no-debsig" as per default.
			
				NOTE2: Signatures from developers are currently stripped when they enter off the package archive since the currently preferred method is release checks as described previously.
			[[47]](#idm3301)
				Some operating systems have already been plagued with automatic-updates problems such as the [http://www.cunap.com/~hardingr/projects/osx/exploit.html](http://www.cunap.com/~hardingr/projects/osx/exploit.html).
			[[48]](#idm3307)
				Older releases, such as Debian 3.1 sarge can use this feature by using backported versions of this package management tool
			[[49]](#idm3362)
					Until an automatic mechanism is developed.
				[[50]](#idm3415)
						Technically speaking, this is an ASCII-armored detached gpg signature.
					[[51]](#idm3434)
						Or has poisoned your DNS, or is spoofing the server, or has replaced the file in the mirror you are using, etc.
					[[52]](#idm3463)
						"ziyi" is the name of the tool used for signing on the Debian servers, the name is based on the name of a [http://en.wikipedia.org/wiki/Zhang_Ziyi](http://en.wikipedia.org/wiki/Zhang_Ziyi).
					[[53]](#idm3480)
						Not all apt repository keys are signed at all by another key. Maybe the person setting up the repository doesn't have another key, or maybe they don't feel comfortable signing such a role key with their main key. For information on setting up a key for a repository see [SectionÂ 7.5.4, âRelease check of non Debian sourcesâ](deb-pack-sign.en.md#check-non-debian-releases).
					[[54]](#idm3514)
						Either because you are using the stable, sarge, release or an older release or because you don't want to use the latest apt version, although we would really appreciate testing of it.
					[Prev7.4.Â Debian Security Build Infrastructure](ch07s04.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 8.Â Security tools in Debian](sec-tools.en.md)