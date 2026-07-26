---
title: "16.1. Upcoming Developments"
source: "https://www.debian.org/doc/manuals/debian-handbook/conclusion.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/conclusion.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "16.1. Upcoming Developments"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.becoming-package-maintainer.en.md)The Debian Administrator's Handbook[Next](sect.future-of-debian.en.md)

# ChapterÂ 16.Â Conclusion: Debian's Future

[16.1. Upcoming Developments](conclusion.en.md#sect.upcoming-developments)[16.2. Debian's Future](sect.future-of-debian.en.md)[16.3. Future of this Book](sect.future-of-this-book.en.md)
		The story of Falcot Corp ends with this last chapter; but Debian lives on, and the future will certainly bring many interesting surprises.
	

## 16.1.Â Upcoming Developments

			Now that Debian versionÂ 11 is out, the developers are already busy working on the next version, codenamed Bookwormâ¦
		
			There is no official list of all planned changes, and Debian never makes promises relating to technical goals of the coming versions. However, a few development trends and discussion topics can already be noted, and we can try to guess what might happen (or not). Some of the expected changes are documented in the Debian 11 release notes: âÂ [https://www.debian.org/releases/bullseye/amd64/release-notes/ch-information.en.html#deprecated-components](https://www.debian.org/releases/bullseye/amd64/release-notes/ch-information.en.html#deprecated-components)

			Beyond usual deprecation of some software components, it is worth pointing out that Debian is in the process of switching to what is known as a merged-usr filesystem: in this scheme /bin, /sbin and /lib are symlinks pointing to the corresponding directories in /usr. This improves compatibility between all Unix systems, makes us closer of having all Debian-supplied files in a single top-level directory that can be easily protected, snapshotted or shared. You can learn more about the benefits here: âÂ [https://www.freedesktop.org/wiki/Software/systemd/TheCaseForTheUsrMerge/](https://www.freedesktop.org/wiki/Software/systemd/TheCaseForTheUsrMerge/)

			This important change is not without creating issues: dpkg will have to learn about those aliased directories, but the dpkg maintainer doesn't like the technical solution deployed by Debian and hasn't yet made the required changes. The Debian technical committee's help has been requested multiple times already. Their last decision can be found here: âÂ [https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=994388#110](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=994388#110)

apt-key will become obsolete. The key management for third party repositories should only rely on keys found in /etc/apt/trusted.gpg.d or configured via Signed-By as described in [SectionÂ 6.6, âChecking Package Authenticityâ](sect.package-authentication.en.md).
		
			For some tasks the default software solution will change. As an example: plocate might be a faster and smaller replacement for mlocate. systemd will continue to add new features that help to standardize the boot process and system management, allowing us to get rid of some other software in the base system.
		
			Of course all the main software suites will have a major release. The latest version of the various desktops will bring better usability and new features.
		
			The default permission of home directories will be more restrictive, allowing only the user to access their files.
		
			Developments which already began will continue: Improve build reproducibility and security for example. With the widespread use of continuous integration and the growth of the archive (and of the biggest packages!), the constraints on release architectures will be harder to meet and architectures will be dropped.
		[Prev15.4.Â Becoming a Package Maintainer](sect.becoming-package-maintainer.en.md)[Up](#)[Home](index.en.md)[Next16.2.Â Debian's Future](sect.future-of-debian.en.md)