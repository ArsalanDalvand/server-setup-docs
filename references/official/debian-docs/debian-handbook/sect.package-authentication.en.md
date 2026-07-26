---
title: "6.6. Checking Package Authenticity"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.package-authentication.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.package-authentication.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "6.6. Checking Package Authenticity"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.apt-frontends.en.md)The Debian Administrator's Handbook[Next](sect.dist-upgrade.en.md)

## 6.6.Â Checking Package Authenticity

			Security is very important for Falcot Corp administrators. Accordingly, they need to ensure that they only install packages which are guaranteed to come from Debian with no tampering on the way. A computer cracker could try to add malicious code to an otherwise legitimate package. Such a package, if installed, could do anything the cracker designed it to do, including for instance disclosing passwords or confidential information. To circumvent this risk, Debian provides a tamper-proof seal to guarantee â at install time â that a package really comes from its official maintainer and hasn't been modified by a third party.
		
			The seal works with a chain of cryptographic hashes and a signature and is explained in detail in apt-secure(8). Starting with Debian 10 Buster the signed file is the InRelease file, provided by the Debian mirrors. There is also a legacy file called Release. Both contain a list of the Packages files (including their compressed forms, Packages.gz and Packages.xz, and the incremental versions), along with their SHA256 hashes, which ensures that the files haven't been tampered with. These Packages files contain a list of the Debian packages available on the mirror, along with their hashes, which ensures in turn that the contents of the packages themselves haven't been altered either. The difference between InRelease and Release is that the former is cryptographically signed in-line, whereas the latter provides a detached signature in the form of the file Release.gpg.
		

NOTE The future of Release and Release.gpg

			After the release of Debian 10 Buster the intention has been announced to remove support for the legacy files Release and Release.gpg in APT, used since version 0.6, which introduced support for an archive authentication.
		
			APT needs a set of trusted GnuPG public keys to verify signatures in the InRelease and Release.gpg files available on the mirrors. It gets them from files in /etc/apt/trusted.gpg.d/ and from the /etc/apt/trusted.gpg keyring (managed by the apt-key command). The official Debian keys are provided and kept up-to-date by the debian-archive-keyring package which puts them in /etc/apt/trusted.gpg.d/:
		

```bash
# ls /etc/apt/trusted.gpg.d/
debian-archive-bullseye-automatic.gpg
debian-archive-bullseye-security-automatic.gpg
debian-archive-bullseye-stable.gpg
debian-archive-buster-automatic.gpg
debian-archive-buster-security-automatic.gpg
debian-archive-buster-stable.gpg
debian-archive-stretch-automatic.gpg
debian-archive-stretch-security-automatic.gpg
debian-archive-stretch-stable.gpg
```

IN PRACTICE Adding trusted keys

			When a third-party package source is added to the sources.list file, APT needs to be told to trust the corresponding GPG authentication key (otherwise it will keep complaining that it can't ensure the authenticity of the packages coming from that repository). The first step is of course to get the public key. More often than not, the key will be provided as a small text file, which we will call key.asc in the following examples.
		
			To add the key to APT's trusted keyring, the administrator can just put it in a *.gpg file in /etc/apt/trusted.gpg.d/ (*.asc files are discouraged and can cause problems). This is supported since Debian Stretch. With older releases, you had to run apt-key add < key.asc, which is considered obsolete already.
		
			While this is very common behavior, it also creates a possible issue. APT does not know which key belongs to which repository. If one of the keys in APT's keyring matches the signature of an archive, APT considers the archive as authenticated. Once a key has been added to the trusted keyring, a malicious attacker could actually provide its own "version" of a Debian mirror signed by the same key. It might therefor actually be better to put the key as *.gpg file into /usr/share/keyrings/ and use the Signed-By field in sources.list for third party repositories as shown in the example below.
		
			Imagine, that the Falcot Corp. runs their own public repository and you want to use it. They have signed it and provide a publicly available key(ring) as well to download. To restrict the key to the repository, you can simply do this:
		

```bash
$ sudo wget -O falcot-keyring.gpg -P /usr/share/keyrings https://packages.falcot.com/debian/repository.gpg
```

```bash
deb [ signed-by=/usr/share/keyrings/falcot-keyring.gpg ] https://packages.falcot.com/debian bullseye main
```

			Beware that adding and using third party repositories always poses a security and stability risk!
		
			Once the appropriate keys are in the keyring, APT will check the signatures before any risky operation, so that frontends will display a warning if asked to install a package whose authenticity can't be ascertained.
		
			Note, that binary packages are usually not signed. The integrity of a package can only be confirmed by checking its hashsums against a trusted (and possibly signed) hashsum source.
		

SPECIFIC CASE Unsigned repositories

			While in general unsigned repositories shouldn't be used, some users might still require them, for example to provide some locally built packages. APT can be forced to accept such a repository by prepending the repository URL using allow-insecure=yes or trusted=yes. We will demonstrate it using the example from [SectionÂ 15.3, âCreating a Package Repository for APTâ](sect.setup-apt-package-repository.en.md):
		

```bash
deb [ trusted=yes ] http://packages.falcot.com/ updates/
deb [ trusted=yes ] http://packages.falcot.com/ internal/
```

[Prev6.5.Â Frontends: aptitude, synaptic](sect.apt-frontends.en.md)[Up](#)[Home](index.en.md)[Next6.7.Â Upgrading from One Stable Distribution to th...](sect.dist-upgrade.en.md)