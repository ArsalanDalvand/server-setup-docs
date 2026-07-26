---
title: "8.1.3. Migrating to UTF-8"
source: "https://www.debian.org/doc/manuals/debian-handbook/basic-configuration.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/basic-configuration.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "8.1.3. Migrating to UTF-8"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.common-procedures.en.md)The Debian Administrator's Handbook[Next](sect.network-config.en.md)

# ChapterÂ 8.Â Basic Configuration: Network, Accounts, Printing...

[8.1. Configuring the System for Another Language](basic-configuration.en.md#sect.config-language-support)[8.1.1. Setting the Default Language](basic-configuration.en.md#sect.default-language)[8.1.2. Configuring the Keyboard](basic-configuration.en.md#sect.keyboard-config)[8.1.3. Migrating to UTF-8](basic-configuration.en.md#sect.utf8-migration)[8.2. Configuring the Network](sect.network-config.en.md)[8.2.1. Ethernet Interface](sect.network-config.en.md#sect.interface-ethernet)[8.2.2. Wireless Interface](sect.network-config.en.md#sect.interface-wireless)[8.2.3. Connecting with PPP through a PSTN Modem](sect.network-config.en.md#sect.ppp-rtc)[8.2.4. Connecting through an ADSL Modem](sect.network-config.en.md#sect.adsl)[8.2.5. Automatic Network Configuration for Roaming Users](sect.network-config.en.md#sect.roaming-network-config)[8.3. Setting the Hostname and Configuring the Name Service](sect.hostname-name-service.en.md)[8.3.1. Name Resolution](sect.hostname-name-service.en.md#sect.name-resolution)[8.4. User and Group Databases](sect.user-group-databases.en.md)[8.4.1. User List: /etc/passwd](sect.user-group-databases.en.md#sect.etc-passwd)[8.4.2. The Hidden and Encrypted Password File: /etc/shadow](sect.user-group-databases.en.md#sect.etc-shadow)[8.4.3. Modifying an Existing Account or Password](sect.user-group-databases.en.md#sect.account-modification)[8.4.4. Disabling an Account](sect.user-group-databases.en.md#sect.disabling-account)[8.4.5. Group List: /etc/group](sect.user-group-databases.en.md#sect.etc-group)[8.5. Creating Accounts](sect.creating-accounts.en.md)[8.6. Shell Environment](sect.shell-environment.en.md)[8.7. Printer Configuration](sect.config-printing.en.md)[8.8. Configuring the Bootloader](sect.config-bootloader.en.md)[8.8.1. Identifying the Disks](sect.config-bootloader.en.md#sect.identify-disks)[8.8.2. GRUBÂ 2 Configuration](sect.config-bootloader.en.md#sect.config-grub)[8.8.3. Using GRUB with EFI and Secure Boot](sect.config-bootloader.en.md#sect.config-grub-efi)[8.9. Other Configurations: Time Synchronization, Logs, Sharing Accessâ¦](sect.config-misc.en.md)[8.9.1. Timezone](sect.config-misc.en.md#sect.timezone)[8.9.2. Time Synchronization](sect.config-misc.en.md#sect.time-synchronization)[8.9.3. Rotating Log Files](sect.config-misc.en.md#sect.rotation-logs)[8.9.4. Sharing Administrator Rights](sect.config-misc.en.md#sect.sharing-admin-rights)[8.9.5. List of Mount Points](sect.config-misc.en.md#sect.fstab-mount-points)[8.9.6. locate and updatedb](sect.config-misc.en.md#sect.locate-updatedb)[8.10. Compiling a Kernel](sect.kernel-compilation.en.md)[8.10.1. Introduction and Prerequisites](sect.kernel-compilation.en.md#sect.kernel-compilation-prerequisites)[8.10.2. Getting the Sources](sect.kernel-compilation.en.md#sect.kernel-sources)[8.10.3. Configuring the Kernel](sect.kernel-compilation.en.md#sect.config-kernel)[8.10.4. Compiling and Building the Package](sect.kernel-compilation.en.md#sect.kernel-build)[8.10.5. Compiling External Modules](sect.kernel-compilation.en.md#sect.modules-build)[8.10.6. Applying a Kernel Patch](sect.kernel-compilation.en.md#sect.kernel-patch)[8.11. Installing a Kernel](sect.kernel-installation.en.md)[8.11.1. Features of a Debian Kernel Package](sect.kernel-installation.en.md#sect.kernel-package)[8.11.2. Installing with dpkg](sect.kernel-installation.en.md#sect.kernel-installation-with-dpkg)
		A computer with a new installation created with debian-installer is intended to be as functional as possible, but many services still have to be configured. Furthermore, it is always good to know how to change certain configuration elements defined during the initial installation process.
	
		This chapter reviews everything included in what we could call the âbasic configurationâ: networking, language and locales, users and groups, printing, mount points, etc.
	

## 8.1.Â Configuring the System for Another Language

			If the system was installed using French, the machine will probably already have French set as the default language. But it is good to know what the installer does to set the language, so that later, if the need arises, you can change it.
		

TOOL The locale command to display the current configuration

			The locale command lists a summary of the current configuration of various locale parameters (date format, numbers format, etc.), presented in the form of a group of standard environment variables dedicated to the dynamic modification of these settings.
		

### 8.1.1.Â Setting the Default Language

				A locale is a group of regional settings. This includes not only the language for text, but also the format for displaying numbers, dates, times, and monetary sums, as well as the alphabetical comparison rules (to properly account for accented characters). Although each of these parameters can be specified independently from the others, we generally use a locale, which is a coherent set of values for these parameters corresponding to a âregionâ in the broadest sense. These locales are usually indicated under the form, language-code_COUNTRY-CODE, sometimes with a suffix to specify the character set and encoding to be used. This enables consideration of idiomatic or typographical differences between different regions with a common language.
			

CULTURE Character sets

				Historically, each locale has an associated âcharacter setâ (group of known characters) and a preferred âencodingâ (internal representation for characters within the computer).
			
				The most popular encodings for latin-based languages were limited to 256 characters because they opted to use a single byte for each character. Since 256 characters was not enough to cover all European languages, multiple encodings were needed, and that is how we ended up with ISO-8859-1 (also known as âLatin 1â) up to ISO-8859-15 (also known as âLatin 9â), among others.
			
				Working with foreign languages often implied regular switches between various encodings and character sets. Furthermore, writing multilingual documents led to further, almost intractable problems. Unicode (a super-catalog of nearly all writing systems from all of the world's languages) was created to work around this problem. One of Unicode's encodings, UTF-8, retains all 128 ASCII symbols (7-bit codes), but handles other characters differently. Those are preceded by a specific escape sequence of a few bits, which implicitly defines the length of the character. This allows encoding all Unicode characters on a sequence of one or more bytes. Its use has been popularized by the fact that it is the default encoding in XML documents.
			
				This is the encoding that should generally be used, and is thus the default on Debian systems.
			
				The locales package includes all the elements required for proper functioning of âlocalizationâ of various applications. During installation, this package will ask you to select a set of supported languages. This set can be changed at any time by running dpkg-reconfigure locales as root.
			
				The first question invites you to select âlocalesâ to support. Selecting all English locales (meaning those beginning with âen_â) is a reasonable choice. Do not hesitate to also enable other locales if the machine will host foreign users. The list of locales enabled on the system is stored in the /etc/locale.gen file. It is possible to edit this file by hand, but you should run locale-gen after any modifications. It will generate the necessary files for the added locales to work, and remove any obsolete files.
			
				The second question, entitled âDefault locale for the system environmentâ, requests a default locale. The recommended choice in the USA is âen_US.UTF-8â. British English speakers will prefer âen_GB.UTF-8â, and Canadians will prefer either âen_CA.UTF-8â or, for French, âfr_CA.UTF-8â. The /etc/default/locale file will then be modified to store this choice. From there, it is picked up by all user sessions since PAM will inject its content in the LANG environment variable.
			
				The locales-all package contains the precompiled locale data for all supported locales.
			

BEHIND THE SCENES /etc/environment and /etc/default/locale

				The /etc/environment file provides the login, gdm, or even ssh programs with the correct environment variables to be created.
			
				These applications do not create these variables directly, but rather via a PAM (pam_env.so) module. PAM (Pluggable Authentication Module) is a modular library centralizing the mechanisms for authentication, session initialization, and password management. See [SectionÂ 11.7.3.2, âConfiguring PAMâ](sect.ldap-directory.en.md#sect.config-pam) for an example of PAM configuration.
			
				The /etc/default/locale file works in a similar manner, but contains only the LANG environment variable. Thanks to this split, some PAM users can inherit a complete environment without localization. Indeed, it is generally discouraged to run server programs with localization enabled; on the other hand, localization and regional settings are recommended for programs that open user sessions.
			

### 8.1.2.Â Configuring the Keyboard

				Even if the keyboard layout is managed differently in console and graphical mode, Debian offers a single configuration interface that works for both: it is based on debconf and is implemented in the keyboard-configuration package. Thus the dpkg-reconfigure keyboard-configuration command can be used at any time to reset the keyboard layout.
			
The questions are relevant to the physical keyboard layout (a standard PC keyboard in the US will be a âGeneric 104 keyâ), then the layout to choose (generally âUSâ), and then the position of the AltGr key (right Alt). Finally comes the question of the key to use for the âCompose keyâ, which allows for entering special characters by combining keystrokes. Type successively Compose ' e and produce an e-acute (âÃ©â). All these combinations are described in the /usr/share/X11/locale/en_US.UTF-8/Compose file (or another file, determined according to the current locale indicated by /usr/share/X11/locale/compose.dir).
			
				Note that the keyboard configuration for graphical mode described here only affects the default layout; the GNOME and KDE Plasma environments, among others, provide a keyboard control panel in their preferences allowing each user to have their own configuration. Some additional options regarding the behavior of some particular keys are also available in these control panels.
			

### 8.1.3.Â Migrating to UTF-8

				The generalization of UTF-8 encoding has been a long awaited solution to numerous difficulties with interoperability, since it facilitates international exchange and removes the arbitrary limits on characters that can be used in a document. The one drawback is that it had to go through a rather difficult transition phase. Since it could not be completely transparent (that is, it could not happen at the same time all over the world), two conversion operations were required: one on file contents, and the other on filenames. Fortunately, the bulk of this migration has been completed and we discuss it largely for reference.
			

CULTURE Mojibake and interpretation errors

				When a text is sent (or stored) without encoding information, it is not always possible for the recipient to know with certainty what convention to use for determining the meaning of a set of bytes. You can usually get an idea by getting statistics on the distribution of values present in the text, but that doesn't always give a definite answer. When the encoding system chosen for reading differs from that used in writing the file, the bytes are misinterpreted, and you get, at best, errors on some characters, or, at worst, something completely illegible.
			
				Thus, if a French text appears normal with the exception of accented letters and certain symbols which appear to be replaced with sequences of characters like âÃÂ©â or ÃÂ¨â or âÃÂ§â, it is probably a file encoded as UTF-8 but interpreted as ISO-8859-1 or ISO-8859-15. This is a sign of a local installation that has not yet been migrated to UTF-8. If, instead, you see question marks instead of accented letters â even if these question marks seem to also replace a character that should have followed the accented letter â it is likely that your installation is already configured for UTF-8 and that you have been sent a document encoded in Western ISO.
			
				So much for âsimpleâ cases. These cases only appear in Western culture, since Unicode (and UTF-8) was designed to maximize the common points with historical encodings for Western languages based on the Latin alphabet, which allows recognition of parts of the text even when some characters are missing.
			
				In more complex configurations, which, for example, involve two environments corresponding to two different languages that do not use the same alphabet, you often get completely illegible results â a series of abstract symbols that have nothing to do with each other. This is especially common with Asian languages due to their numerous languages and writing systems. The Japanese word mojibake has been adopted to describe this phenomenon. When it appears, diagnosis is more complex and the simplest solution is often to simply migrate to UTF-8 on both sides.
			
				As far as file names are concerned, the migration can be relatively simple. The convmv tool (in the package with the same name) was created specifically for this purpose; it allows renaming files from one encoding to another. The use of this tool is relatively simple, but we recommend doing it in two steps to avoid surprises. The following example illustrates a UTF-8 environment containing directory names encoded in ISO-8859-15, and the use of convmv to rename them.
			

```bash
$ ls travail/
Ic?nes  ?l?ments graphiques  Textes
$ convmv -r -f iso-8859-15 -t utf-8 travail/
Starting a dry run without changes...
mv "travail/ï¿½lï¿½ments graphiques"        "travail/ÃlÃ©ments graphiques"
mv "travail/Icï¿½nes"     "travail/IcÃ´nes"
No changes to your files done. Use --notest to finally rename the files.
$ convmv -r --notest -f iso-8859-15 -t utf-8 travail/
mv "travail/ï¿½lï¿½ments graphiques"        "travail/ÃlÃ©ments graphiques"
mv "travail/Icï¿½nes"     "travail/IcÃ´nes"
Ready!
$ ls travail/
ÃlÃ©ments graphiques  IcÃ´nes  Textes
```

				For the file content, conversion procedures are more complex due to the vast variety of existing file formats. Some file formats include encoding information that facilitates the tasks of the software used to treat them; it is sufficient, then, to open these files and re-save them specifying UTF-8 encoding. In other cases, you have to specify the original encoding (ISO-8859-1 or âWesternâ, or ISO-8859-15 or âWestern (Euro)â, according to the formulations) when opening the file.
			
				For simple text files, you can use recode (in the package of the same name) which allows automatic recoding. This tool has numerous options so you can play with its behavior. We recommend you consult the documentation, the recode(1) man page, or the recode info page (more complete). Alternatively, iconv supports more character sets, but has less options.
			[Prev7.2.Â Common Procedures](sect.common-procedures.en.md)[Up](#)[Home](index.en.md)[Next8.2.Â Configuring the Network](sect.network-config.en.md)