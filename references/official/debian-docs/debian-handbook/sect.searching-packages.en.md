---
title: "6.10. Searching for Packages"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.searching-packages.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.searching-packages.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "6.10. Searching for Packages"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.automatic-upgrades.en.md)The Debian Administrator's Handbook[Next](solving-problems.en.md)

## 6.10.Â Searching for Packages

			With the large and ever-growing amount of software in Debian, there emerges a paradox: Debian usually has a tool for most tasks, but that tool can be very difficult to find among the myriad other packages. The lack of appropriate ways to search for (and to find) the right tool has long been a problem. Fortunately, this problem has almost entirely been solved.
		
			The most trivial search possible is looking up an exact package name. If apt show package returns a result, then the package exists. Unfortunately, this requires knowing or even guessing the package name, which isn't always possible.
		

TIP Package naming conventions

			Some categories of packages are named according to a conventional naming scheme; knowing the scheme can sometimes allow you to guess exact package names. For instance, for Perl modules, the convention says that a module called XML::Handler::Composer upstream should be packaged as libxml-handler-composer-perl. The library enabling the use of the gconf system from Python is packaged as python-gconf. It is unfortunately not possible to define a fully general naming scheme for all packages, even though package maintainers usually try to follow the choice of the upstream developers.
		
			A slightly more successful searching pattern is a plain-text search in package names, but it remains very limited. You can generally find results by searching package descriptions: since each package has a more or less detailed description in addition to its package name, a keyword search in these descriptions will often be useful. apt-cache and axi-cache are the tools of choice for this kind of search (see [ALTERNATIVE axi-cache](sect.apt-cache.en.md#sidebar.apt-axi-cache)); for instance, apt-cache search video will return a list of all packages whose name or description contains the keyword âvideoâ.
		
			For more complex searches, a more powerful tool such as aptitude is required. aptitude allows you to search according to a logical expression based on the package's meta-data fields. For instance, the following command searches for packages whose name contains kino, whose description contains video and whose maintainer's name contains paul:
		

```bash
$ aptitude search kino~dvideo~mpaul
p   kino  - Non-linear editor for Digital Video data
$ aptitude show kino
Package: kino                     
Version: 1.3.4+dfsg0-1.1
State: not installed
Priority: optional
Section: video
Maintainer: Paul Brossier <piem@debian.org>
Architecture: amd64
Uncompressed Size: 8,316 k
Depends: libasound2 (>= 1.0.16), libavc1394-0 (>= 0.5.3), libavcodec58 (>= 7:4.2),
         libavformat58 (>= 7:4.2), libavutil56 (>= 7:4.0), libc6 (>= 2.29), libdv4
         (>= 1.0.0), libgcc-s1 (>= 3.0), libgdk-pixbuf-2.0-0 (>= 2.22.0),
         libglade2-0 (>= 1:2.6.4-2~), libglib2.0-0 (>= 2.12.0), libgtk2.0-0 (>=
         2.24.0), libiec61883-0 (>= 1.2.0), libpango-1.0-0 (>= 1.14.0),
         libpangoft2-1.0-0 (>= 1.14.0), libquicktime2 (>= 2:1.2.2), libraw1394-11
         (>= 2.1.2), libsamplerate0 (>= 0.1.7), libstdc++6 (>= 9), libswscale5 (>=
         7:4.0), libx11-6, libxext6, libxml2 (>= 2.7.4), libxv1
Recommends: ffmpeg, curl
Suggests: udev | hotplug, vorbis-tools, sox, mjpegtools, lame, ffmpeg2theora
Conflicts: kino-dvtitler, kino-timfx, kinoplus, kino-dvtitler:i386,
           kino-timfx:i386, kinoplus:i386, kino:i386
Replaces: kino-dvtitler, kino-timfx, kinoplus, kino-dvtitler:i386,
          kino-timfx:i386, kinoplus:i386
Provides: kino-dvtitler, kino-timfx, kinoplus
Description: Non-linear editor for Digital Video data
 Kino allows you to record, create, edit, and play movies recorded with DV
 camcorders. This program uses many keyboard commands for fast navigating and
 editing inside the movie. 
 
 The kino-timfx, kino-dvtitler and kinoplus sets of plugins, formerly distributed
 as separate packages, are now provided with Kino.
Homepage: http://www.kinodv.org/
Tags: field::arts, hardware::camera, implemented-in::c, implemented-in::c++,
      interface::graphical, interface::x11, role::program, scope::application,
      suite::gnome, uitoolkit::gtk, use::editing, use::learning,
      works-with::video, x11::application
```

			The search only returns one package, kino, which satisfies all three criteria.
		
			Even these multi-criteria searches are rather unwieldy, which explains why they are not used as much as they could. A new tagging system has therefore been developed, and it provides a new approach to searching. Packages are given tags that provide a thematic classification along several strands, known as a âfacet-based classificationâ. In the case of kino above, the package's tags indicate that Kino is a Gnome-based software that works on video data and whose main purpose is editing.
		
			Browsing this classification can help you to search for a package which corresponds to known needs; even if it returns a (moderate) number of hits, the rest of the search can be done manually. To do that, you can use the ~G search pattern in aptitude, but it is probably easier to simply navigate the site where tags are managed or use the debtags command: âÂ [https://debtags.debian.org/](https://debtags.debian.org/)

```bash
$ debtags search "works-with::video && use::editing"
```

			Selecting the works-with::video and use::editing tags yields a handful of packages, including the kino and pitivi video editors. This system of classification is bound to be used more and more as time goes on, and package managers will gradually provide efficient search interfaces based on it.
		
			To sum up, the best tool for the job depends on the complexity of the search that you wish to do:
		
apt-cache only allows searching in package names and descriptions, which is very convenient when looking for a particular package that matches a few target keywords;
				
					when the search criteria also include relationships between packages or other meta-data such as the name of the maintainer, synaptic will be more useful;
				
					when a tag-based search is needed, a good tool is packagesearch, a graphical interface dedicated to searching available packages along several criteria (including the names of the files that they contain). For usage on the command-line, axi-cache will fit the bill.
				
					finally, when the searches involve complex expressions with logic operations, the tool of choice will be aptitude's search pattern syntax, which is quite powerful despite being somewhat obscure; it works in both the command-line and the interactive modes.
				[Prev6.9.Â Automatic Upgrades](sect.automatic-upgrades.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 7.Â Solving Problems and Finding Relevant ...](solving-problems.en.md)