---
title: "4.2.2. Security update of the kernel"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/security-update.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/security-update.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "4.2.2. Security update of the kernel"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch04.en.md)Securing Debian Manual[Next](bios-boot.en.md)

## 4.2.Â Execute a security update

			As soon as new security bugs are detected in packages, Debian maintainers and upstream authors generally patch them within days or even hours. After the bug is fixed, a new package is provided on [http://security.debian.org](http://security.debian.org).
		
			If you are installing a Debian release you must take into account that since the release was made there might have been security updates after it has been determined that a given package is vulnerable. Also, there might have been minor releases (there have been four for the Debian 3.0 sarge release) which include these package updates.
		
			During installation security updates are configured for your system and pending updates downloaded and applied, unless you specifically opt out of this or the system was not connected to the Internet. The updates are applied even before the first boot, so the new system starts its life as up to date as possible.
		
			To manually update the system, put the following line in your sources.list and you will get security updates automatically, whenever you update your system. Replace [CODENAME] with the release codename, e.g. squeeze. 

```bash
deb http://security.debian.org/ [CODENAME]/updates main contrib non-free
```

Note: If you are using the testing branch use the security testing mirror sources as described in [SectionÂ 10.1.4, âSecurity support for the testing branchâ](ch10.en.md#security-support-testing).
		
			Once you've done this you can use multiple tools to upgrade your system. If you are running a desktop system you will have[[9]](#ftn.idm831) an application called update-notifier that will make it easy to check if new updates are available, by selecting it you can make a system upgrade from the desktop (using update-manager). For more information see [SectionÂ 10.1.2.2, âChecking for updates at the Desktopâ](ch10.en.md#update-desktop). In desktop environments you can also use synaptic (GNOME), kpackage or adept (KDE) for more advanced interfaces. If you are running a text-only terminal you can use aptitude, apt or dselect (deprecated) to upgrade: 
			
						If you want to use aptitude's text interface you just have to press u (update) followed by g (to upgrade). Or just do the following from the command line (as root): 

```bash
# aptitude update
# aptitude upgrade
```

						If you want to use apt do just like with aptitude but substitute the aptitude lines above with apt-get.
					
						If you want to use dselect then first [U]pdate, then [I]nstall and finally, [C]onfigure the installed/upgraded packages.
					

			If you like, you can add the deb-src lines to /etc/apt/sources.list as well. See apt(8) for further details.
		

### 4.2.1.Â Security update of libraries

				Once you have executed a security update you might need to restart some of the system services. If you do not do this, some services might still be vulnerable after a security upgrade. The reason for this is that daemons that are running before an upgrade might still be using the old libraries before the upgrade [[10]](#ftn.idm866).
			
				From Debian Jessie and up, you can install the needrestart package, which will run automatically after each APT upgrade and prompt you to restart services that are affected by the just-installed updates. In earlier releases, you can run the checkrestart program (available in the debian-goodies package) manually after your APT upgrade.
			
				Some packages (like libc6) will do this check in the postinst phase for a limited set of services specially since an upgrade of essential libraries might break some applications (until restarted)[[11]](#ftn.idm875).
			
				Bringing the system to run level 1 (single user) and then back to run level 3 (multi user) should take care of the restart of most (if not all) system services. But this is not an option if you are executing the security upgrade from a remote connection (like ssh) since it will be severed.
			
				Excercise caution when dealing with security upgrades if you are doing them over a remote connection like ssh. A suggested procedure for a security upgrade that involves a service restart is to restart the SSH daemon and then, immediately, attempt a new ssh connection without breaking the previous one. If the connection fails, revert the upgrade and investigate the issue.
			

### 4.2.2.Â Security update of the kernel

				First, make sure your kernel is being managed through the packaging system. If you have installed using the installation system from Debian 3.0 or previous releases, your kernel is not integrated into the packaging system and might be out of date. You can easily confirm this by running: 

```bash
$ dpkg -S `readlink -f /vmlinuz`
linux-image-2.6.18-4-686: /boot/vmlinuz-2.6.18-4-686
```

				If your kernel is not being managed you will see a message saying that the package manager did not find the file associated to any package instead of the message above, which says that the file associated to the current running kernel is being provided by the linux-image-2.6.18-4-686. So first, you will need to manually install a kernel image package. The exact kernel image you need to install depends on your architecture and your prefered kernel version. Once this is done, you will be able to manage the security updates of the kernel just like those of any other package. In any case, notice that the kernel updates will only be done for kernel updates of the same kernel version you are using, that is, apt will not automatically upgrade your kernel from the 2.4 release to the 2.6 release (or from the 2.4.26 release to the 2.4.27 release[[12]](#ftn.idm889)).
			
				The installation system of recent Debian releases will handle the selected kernel as part of the package system. You can review which kernels you have installed by running: 

```bash
$ COLUMNS=150 dpkg -l 'linux-image*' | awk '$1 ~ /ii/ { print $0 }'
```

				To see if your kernel needs to be updated run: 

```bash
$ kernfile=`readlink -f /vmlinuz`
$ kernel=`dpkg -S $kernfile | awk -F : '{print $1}'`
$ apt-cache policy $kernel
linux-image-2.6.18-4-686:
  Installed: 2.6.18.dfsg.1-12
  Candidate: 2.6.18.dfsg.1-12
  Version table:
 *** 2.6.18.dfsg.1-12 0
        100 /var/lib/dpkg/status
```

				If you are doing a security update which includes the kernel image you need to reboot the system in order for the security update to be useful. Otherwise, you will still be running the old (and vulnerable) kernel image.
			
				If you need to do a system reboot (because of a kernel upgrade) you should make sure that the kernel will boot up correctly and network connectivity will be restored, specially if the security upgrade is done over a remote connection like ssh. For the former you can configure your boot loader to reboot to the original kernel in the event of a failure (for more detailed information read [Remotely rebooting Debian GNU/Linux machines](http://www.debian-administration.org/?article=70)). For the latter you have to introduce a network connectivity test script that will check if the kernel has started up the network subsystem properly and reboot the system if it did not[[13]](#ftn.idm900). This should prevent nasty surprises like updating the kernel and then realizing, after a reboot, that it did not detect or configure the network hardware properly and you need to travel a long distance to bring the system up again. Of course, having the system serial console [[14]](#ftn.idm905) in the system connected to a console or terminal server should also help debug reboot issues remotely.
			[[9]](#idm831)
				In Etch and later releases
			[[10]](#idm866)
					Even though the libraries have been removed from the filesystem the inodes will not be cleared up until no program has an open file descriptor pointing to them.
				[[11]](#idm875)
					This happened, for example, in the upgrade from libc6 2.2.x to 2.3.x due to NSS authentication issues, see [http://lists.debian.org/debian-glibc/2003/03/msg00276.html](http://lists.debian.org/debian-glibc/2003/03/msg00276.html).
				[[12]](#idm889)
					Unless you have installed a kernel metapackage like linux-image-2.6-686 which will always pull in the latest kernel minor revision for a kernel release and a given architecture.
				[[13]](#idm900)
					A sample script called [testnet](http://www.debian-administration.org/articles/70/testnet) is available in the [Remotely rebooting Debian GNU/Linux machines](http://www.debian-administration.org/?article=70) article. A more elaborate network connectivity testing script is available in this [Testing network connectivity article.](http://www.debian-administration.org/?article=128)
[[14]](#idm905)
					Setting up a serial console is beyond the scope of this document, for more information read the [Serial HOWTO](http://www.tldp.org/HOWTO/Serial-HOWTO.html) and the [Remote Serial Console HOWTO](http://www.tldp.org/HOWTO/Remote-Serial-Console-HOWTO/index.html).
				[PrevChapterÂ 4.Â After installation](ch04.en.md)[Up](#)[Home](index.en.md)[Next4.3.Â Change the BIOS (again)](bios-boot.en.md)