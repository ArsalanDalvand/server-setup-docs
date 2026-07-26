---
title: "5.3.2. Usage within Debian"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.source-package-structure.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.source-package-structure.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "5.3.2. Usage within Debian"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.package-meta-information.en.md)The Debian Administrator's Handbook[Next](sect.manipulating-packages-with-dpkg.en.md)

## 5.3.Â Structure of a Source Package

### 5.3.1.Â Format

				A source package is usually comprised of three files, a .dsc, a .orig.tar.gz, and a .debian.tar.xz (or .diff.gz). They allow creation of binary packages (.deb files described above) from the source code files of the program, which are written in a programming language.
			
				The .dsc (Debian Source Control) file is a short text file containing an RFC 2822 header (just like the control file studied in [SectionÂ 5.2.1, âDescription: the control Fileâ](sect.package-meta-information.en.md#sect.control)) which describes the source package and indicates which other files are part thereof. It is signed by its maintainer, which guarantees authenticity. See [SectionÂ 6.6, âChecking Package Authenticityâ](sect.package-authentication.en.md) for further details on this subject.
			

ExampleÂ 5.1.Â A .dsc file

```bash
-----BEGIN PGP SIGNED MESSAGE-----
Hash: SHA512

Format: 3.0 (quilt)
Source: zim
Binary: zim
Architecture: all
Version: 0.73.5-1
Maintainer: Zim Package Maintainers <zim@packages.debian.org>
Uploaders: RaphaÃ«l Hertzog <hertzog@debian.org>
Homepage: https://zim-wiki.org
Standards-Version: 4.5.1
Vcs-Browser: https://salsa.debian.org/debian/zim
Vcs-Git: https://salsa.debian.org/debian/zim.git
Build-Depends: debhelper-compat (= 13), python3, python3-gi, python3-xdg, gir1.2-gtk-3.0, dh-python
Package-List:
 zim deb x11 optional arch=all
Checksums-Sha1:
 80d43d5c1c6a47c695079eb02bc8ad36b84d6e57 2159901 zim_0.73.5.orig.tar.gz
 b1cd86dc4819a80126efbf6ee6eba17a33f451d3 10124 zim_0.73.5-1.debian.tar.xz
Checksums-Sha256:
 a36f15d92c3994c0d55b07f83253b3d8b826beb3714865edbabc14f1cc91d63a 2159901 zim_0.73.5.orig.tar.gz
 6c2db642d9ac1c2440ed08e0cd584006045b342b255f37ffe42bd5459fb5cb76 10124 zim_0.73.5-1.debian.tar.xz
Files:
 fa76ceb8ac7d7354fb0e2bc5607e9faa 2159901 zim_0.73.5.orig.tar.gz
 a0c824d979efb196cde0176d3cb9c719 10124 zim_0.73.5-1.debian.tar.xz

-----BEGIN PGP SIGNATURE-----
Comment: Signed by Raphael Hertzog

iQEzBAEBCgAdFiEE1823g1EQnhJ1LsbSA4gdq+vCmrkFAmAa3ooACgkQA4gdq+vC
mrkq1gf/cs7irmbCSDrADVqsqYBrFJ1FyprE3jiHLNs0OQLryhFj9tzDuilX35VE
HkCfxSaKkzgvQLYtpuw1VBfhOdngTdHO39U6eljkaScnfLWU8Z5n/q+YeedxItoY
X3TtzMexFmb4WJqlylfjbXeqbLdYvsILQ3NVnE48AzkaBQlCC2d9bqecZhWiKfzq
gNxIDVDDhqCXMPe7QCErCBiFPUVpGN7b+6QWN0RxOTLZdj/slRD73rT++VmY+xN1
L8BSLcjXie+ES11MhQNYaLpCv2vqImlZaxkFWvsKBo9ndRFSbE3/RNK479a4KGve
KrdpGUJXy9uLPuAMyn5WphwXJ7OZXQ==
=YFDk
-----END PGP SIGNATURE-----
```

				Note that the source package also has dependencies (Build-Depends) completely distinct from those of binary packages, since they indicate tools required to compile the software in question and construct its binary package.
			

CAUTION Distinct namespaces

				It is important to note here that there is no required correspondence between the name of the source package and that of the binary package(s) that it generates. It is easy enough to understand if you know that each source package may generate several binary packages. This is why the .dsc file has the Source and Binary fields to explicitly name the source package and store the list of binary packages that it generates.
			

CULTURE Why divide into several packages

				Quite frequently, a source package (for a given software) can generate several binary packages. The split is justified by the possibility to use (parts of) the software in different contexts. Consider a shared library, it may be installed to make an application work (for example, libc6), or it can be installed to develop a new program (libc6-dev will then be the correct package). We find the same logic for client/server services where we want to install the server part on one machine and the client part on others (this is the case, for example, of openssh-server and openssh-client).
			
				Just as frequently, the documentation is provided in a dedicated package: the user may install it independently from the software, and may at any time choose to remove it to save disk space. Additionally, this also saves disk space on the Debian mirrors, since the documentation package will be shared among all of the architectures (instead of having the documentation duplicated in the packages for each architecture).
			

PERSPECTIVE Different source package formats

				Originally there was only one source package format. This is the 1.0 format, which associates an .orig.tar.gz archive to a .diff.gz âdebianizationâ patch (there is also a variant, consisting of a single .tar.gz archive, which is automatically used if no .orig.tar.gz is available).
			
				Since Debian 6 Squeeze, Debian developers have the option to use new formats that correct many problems of the historical format. Format 3.0 (quilt) can combine multiple upstream archives in the same source package: in addition to the usual .orig.tar.gz, supplementary .orig-component.tar.gz archives can be included. This is useful with software that is distributed in several upstream components but for which a single source package is desired. These archives can also be compressed with xz rather than gzip, which saves disk space and network resources. Finally, the monolithic patch, .diff.gz is replaced by a .debian.tar.xz archive containing the compiling instructions and a set of upstream patches contributed by the package maintainer. These last are recorded in a format compatible with quilt â a tool that facilitates the management of a series of patches.
			
				The .orig.tar.gz file is an archive containing the source code as provided by the original developer. Debian package maintainers are asked to not modify this archive in order to be able to easily check the origin and integrity of the file (by simple comparison with a checksum) and to respect the wishes of some authors.
			
				The .debian.tar.xz contains all of the modifications made by the Debian maintainer, especially the addition of a debian directory containing the instructions to execute to construct one or more Debian binary packages.
			

TOOL Decompressing a source package

				If you have a source package, you can use the dpkg-source command (from the dpkg-dev package) to decompress it:
			

```bash
$ dpkg-source -x zim_0.73.5-1.dsc
dpkg-source: info: extracting zim in zim-0.73.5
dpkg-source: info: unpacking zim_0.73.5.orig.tar.gz
dpkg-source: info: unpacking zim_0.73.5-1.debian.tar.xz
```

				You can also use apt to download a source package and unpack it right away. It requires that the appropriate deb-src lines be present in the /etc/apt/sources.list file, however (for further details, see [SectionÂ 6.1, âFilling in the sources.list Fileâ](apt.en.md#sect.apt-sources.list)). These are used to list the âsourcesâ of source packages (meaning the servers on which a group of source packages are hosted).
			

```bash
$ apt source package

Reading package lists... Done
NOTICE: 'zim' packaging is maintained in the 'Git' version control system at:
https://salsa.debian.org/debian/zim.git
Please use:
git clone https://salsa.debian.org/debian/zim.git
to retrieve the latest (possibly unreleased) updates to the package.
Need to get 2,172 kB of source archives.
Get:1 https://deb.debian.org/debian bullseye/main zim 0.73.5-1 (dsc) [1,580 B]
Get:2 https://deb.debian.org/debian bullseye/main zim 0.73.5-1 (tar) [2,160 kB]
Get:3 https://deb.debian.org/debian bullseye/main zim 0.73.5-1 (diff) [10.1 kB]
Fetched 2,172 kB in 0s (7,176 kB/s)
dpkg-source: info: extracting zim in zim-0.73.5
dpkg-source: info: unpacking zim_0.73.5.orig.tar.gz
dpkg-source: info: unpacking zim_0.73.5-1.debian.tar.xz
```

### 5.3.2.Â Usage within Debian

				The source package is the foundation of everything in Debian. All Debian packages come from a source package, and each modification in a Debian package is the consequence of a modification made to the source package. The Debian maintainers work with the source package, knowing, however, the consequences of their actions on the binary packages. The fruits of their labors are thus found in the source packages available from Debian: you can easily go back to them and everything stems from them. [ChapterÂ 15, Creating a Debian Package](debian-packaging.en.md) contains some examples.
			
				When a new version of a source package arrives on the Debian server, it will then be used by a network of machines of different architectures for compilation on the various architectures supported by Debian. âÂ [https://buildd.debian.org/](https://buildd.debian.org/)

GOING FURTHER Source only maintainer uploads

				Right after the release of Debian 10 Buster the [Release Team](sect.release-lifecycle.en.md#srm-team) announced that maintainer binary uploads will no longer be accepted for main and all binary packages in this component must be built automatically from mandatory source-only uploads. While binary uploads are still allowed for the contrib and non-free components, we will not focus on this possibility as it should only be the exception.
			[Prev5.2.Â Package Meta-Information](sect.package-meta-information.en.md)[Up](#)[Home](index.en.md)[Next5.4.Â Manipulating Packages with dpkg](sect.manipulating-packages-with-dpkg.en.md)