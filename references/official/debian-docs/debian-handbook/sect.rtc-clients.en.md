---
title: "13.9. Real-Time Communications software"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.rtc-clients.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.rtc-clients.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "13.9. Real-Time Communications software"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.windows-emulation.en.md)The Debian Administrator's Handbook[Next](security.en.md)

## 13.9.Â Real-Time Communications software

			Debian provides a wide range of Real-Time Communications (RTC) client software. The setup of RTC servers is discussed in [SectionÂ 11.8, âReal-Time Communication Servicesâ](sect.rtc-services.en.md). In SIP (Session Initiation Protocol) terminology, a client application or device is also referred to as a user agent.
		
			Each client application varies in functionality. Some applications are more convenient for intensive chat users while other applications are more stable for webcam users. It may be necessary to test several applications to identify those which are most satisfactory. A user may finally decide that they need more than one application, for example, an XMPP application for messaging with customers and an IRC application for collaboration with some online communities.
		
			To maximize the ability of users to communicate with the wider world, it is recommended to configure both SIP and XMPP clients or a single client that supports both protocols.
		
			The default GNOME desktop suggests the Empathy communications client. Empathy can support both SIP and XMPP. It supports instant messaging (IM), voice and video. The KDE project provides KDE Telepathy, a communications client based on the same underlying Telepathy APIs used by the GNOME Empathy client.
		
			Popular alternatives to Empathy/Telepathy include Ekiga, Linphone, Psi, Jitsi and Jami (formerly known as Ring).
		
			Jitsi requires no client other than a web browser. It support encrypted calls, and there is a huge list of public instances. There is no official Debian package for Jitsi, but it can be downloaded from their official website.
		
			Some of these applications can also interact with mobile users using apps such as Lumicall on Android. âÂ [https://lumicall.org](https://lumicall.org)

			The Real-Time Communications Quick Start Guide has a chapter dedicated to client software. âÂ [http://rtcquickstart.org/guide/multi/useragents.html](http://rtcquickstart.org/guide/multi/useragents.html)

TIP Look for clients supporting ICE and TURN

			Some RTC clients have significant problems sending voice and video through firewalls and NAT networks. Users may receive ghost calls (their phone rings but they don't hear the other person) or they may not be able to call at all.
		
			The ICE and TURN protocols were developed to resolve these issues. Operating a TURN server with public IP addresses in each site and using client software that supports both ICE and TURN gives the best user experience.
		
			If the client software is only intended for instant messaging, there is no requirement for ICE or TURN support.
		

ALTERNATIVE Internet Relay Chat

			IRC can also be considered, in addition to SIP and XMPP. IRC is more oriented around the concept of channels, the name of which starts with a hash sign #. Each channel is usually targeted at a specific topic and any number of people can join a channel to discuss it (but users can still have one-to-one private conversations if needed). The IRC protocol is older, and does not allow end-to-end encryption of the messages; it is still possible to encrypt the communications between the users and the server by tunneling the IRC protocol inside SSL.
		
			IRC clients are a bit more complex, and they usually provide many features that are of limited use in a corporate environment. For instance, channel âoperatorsâ are users endowed with the ability to kick other users from a channel, or even ban them permanently, when the normal discussion is disrupted.
		
			Since the IRC protocol is very old, many clients are available to cater for many user groups; examples include XChat, and Smuxi (graphical clients based on GTK+), Irssi (text mode), Circe (integrated to Emacs), and so on.
		[Prev13.8.Â Emulating Windows: Wine](sect.windows-emulation.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 14.Â Security](security.en.md)