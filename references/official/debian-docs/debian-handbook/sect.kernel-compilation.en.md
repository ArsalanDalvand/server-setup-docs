---
title: "8.10.6. Applying a Kernel Patch"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.kernel-compilation.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.kernel-compilation.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "8.10.6. Applying a Kernel Patch"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.config-misc.en.md)The Debian Administrator's Handbook[Next](sect.kernel-installation.en.md)

## 8.10.Â Compiling a Kernel

			The kernels provided by Debian include the largest possible number of features, as well as the maximum of drivers, in order to cover the broadest spectrum of existing hardware configurations. This is why some users prefer to recompile the kernel in order to only include what they specifically need. There are two reasons for this choice. First, it may be to optimize memory consumption, since the kernel code, even if it is never used, occupies memory for nothing (and never âgoes downâ on the swap space, since it is actual RAM that it uses), which can decrease overall system performance. A locally compiled kernel can also limit the risk of security problems since only a fraction of the kernel code is compiled and run.
		

NOTE Security updates

			If you choose to compile your own kernel, you must accept the consequences: Debian cannot ensure security updates for your custom kernel. By keeping the kernel provided by Debian, you benefit from updates prepared by the Debian Project's security team.
		
			Recompilation of the kernel is also necessary if you want to use certain features that are only available as patches (and not included in the standard kernel version).
		

GOING FURTHER The Debian Kernel Handbook

			The Debian kernel team maintains the âDebian Kernel Handbookâ (also available in the debian-kernel-handbook package) with comprehensive documentation about most kernel related tasks and about how official Debian kernel packages are handled. This is the first place you should look into if you need more information than what is provided in this section. âÂ [https://kernel-team.pages.debian.net/kernel-handbook/](https://kernel-team.pages.debian.net/kernel-handbook/)

### 8.10.1.Â Introduction and Prerequisites

				Unsurprisingly Debian manages the kernel in the form of a package, which is not how kernels have traditionally been compiled and installed. Since the kernel remains under the control of the packaging system, it can then be removed cleanly, or deployed on several machines. Furthermore, the scripts associated with these packages automate the interaction with the bootloader and the initrd generator.
			
				The upstream Linux sources contain everything needed to build a Debian package of the kernel. But you still need to install build-essential to ensure that you have the tools required to build a Debian package. Furthermore, the configuration step for the kernel requires the libncurses-dev package (formerly libncurses5-dev, which is now a transitional package). Finally, the fakeroot package will enable creation of the Debian package without using administrator's rights.
			

CULTURE The good old days of kernel-package

				Before the Linux build system gained the ability to build proper Debian packages, the recommended way to build such packages was to use make-kpkg from the kernel-package package.
			

### 8.10.2.Â Getting the Sources

				Like anything that can be useful on a Debian system, the Linux kernel sources are available in a package. To retrieve them, just install the linux-source-version package. The apt search ^linux-source command lists the various kernel versions packaged by Debian. The latest version is available in the Unstable distribution: you can retrieve them without much risk (especially if your APT is configured according to the instructions of [SectionÂ 6.2.6, âWorking with Several Distributionsâ](sect.apt-get.en.md#sect.apt-mix-distros)). Note that the source code contained in these packages does not correspond precisely with that published by Linus Torvalds and the kernel developers; like all distributions, Debian applies a number of patches, which might (or might not) find their way into the upstream version of Linux. These modifications include backports of fixes/features/drivers from newer kernel versions, new features not yet (entirely) merged in the upstream Linux tree, and sometimes even Debian specific changes.
			
				The remainder of this section focuses on the 5.10 version of the Linux kernel, but the examples can, of course, be adapted to the particular version of the kernel that you want.
			
				We assume the linux-source-5.10 package has been installed. It contains /usr/src/linux-source-5.10.tar.xz, a compressed archive of the kernel sources. You must extract these files in a new directory (not directly under /usr/src/, since there is no need for special permissions to compile a Linux kernel): ~/kernel/ is appropriate.
			

```bash
$ mkdir ~/kernel; cd ~/kernel
$ tar -xaf /usr/src/linux-source-5.10.tar.xz
```

CULTURE Location of kernel sources

				Traditionally, Linux kernel sources would be placed in /usr/src/linux/ thus requiring root permissions for compilation. However, working with administrator rights should be avoided when not needed. There is a src group that allows members to work in this directory, but working in /usr/src/ should be avoided, nevertheless. By keeping the kernel sources in a personal directory, you get security on all counts: no files in /usr/ unknown to the packaging system, and no risk of misleading programs that read /usr/src/linux when trying to gather information on the used kernel.
			
				To build a kernel from the pristine sources, just download the tarball of the version of your choice from kernel.org, verify the integrity after importing the kernel maintainers key, and then proceed as described in the following chapters. âÂ [https://kernel.org/pub/linux/kernel/](https://kernel.org/pub/linux/kernel/) âÂ [https://git.kernel.org/pub/scm/docs/kernel/pgpkeys.git/tree/keys](https://git.kernel.org/pub/scm/docs/kernel/pgpkeys.git/tree/keys)

```bash
$ wget https://kernel.org/pub/linux/kernel/v5.x/linux-5.10.62.tar.xz
[..]
$ wget https://kernel.org/pub/linux/kernel/v5.x/linux-5.10.62.tar.sign
[..]
$ unxz -c linux-5.10.62.tar.xz | gpg --verify linux-5.10.62.tar.sign -
gpg: Signature made Fri 03 Sep 2021 10:11:35 AM CEST
gpg:                using RSA key 647F28654894E3BD457199BE38DBBDC86092693E
gpg: Good signature from "Greg Kroah-Hartman <gregkh@linuxfoundation.org>" [unknown]
gpg:                 aka "Greg Kroah-Hartman (Linux kernel stable release signing key) <greg@kroah.com>" [unknown]
gpg:                 aka "Greg Kroah-Hartman <gregkh@kernel.org>" [unknown]
gpg: WARNING: This key is not certified with a trusted signature!
gpg:          There is no indication that the signature belongs to the owner.
Primary key fingerprint: 647F 2865 4894 E3BD 4571  99BE 38DB BDC8 6092 693E
```

### 8.10.3.Â Configuring the Kernel

				The next step consists of configuring the kernel according to your needs. The exact procedure depends on the goals.
			
				When recompiling a more recent version of the kernel (possibly with an additional patch), the configuration will most likely be kept as close as possible to that proposed by Debian. In this case, and rather than reconfiguring everything from scratch, it is sufficient to copy the /boot/config-version file (the version is that of the kernel currently used, which can be found with the uname -r command) into a .config file in the directory containing the kernel sources. Make sure to read sidebar [TIP Missing debian/certs/debian-uefi-certs.pem](sect.kernel-compilation.en.md#sidebar.kernel-config-keyring) in this case.
			

```bash
$ cp /boot/config-5.10.0-8-amd64 ~/kernel/linux-source-5.10/.config
```

				Unless you need to change the configuration, you can stop here and skip to [SectionÂ 8.10.4, âCompiling and Building the Packageâ](sect.kernel-compilation.en.md#sect.kernel-build). If you need to change it, on the other hand, or if you decide to reconfigure everything from scratch, you must take the time to configure your kernel. There are various dedicated interfaces in the kernel source directory that can be used by calling the make target command, where target is one of the values described below.
			
make menuconfig compiles and executes a text-mode interface (this is where the libncurses-dev package is required) which allows navigating the options available in a hierarchical structure. Pressing the Space key changes the value of the selected option, and Enter validates the button selected at the bottom of the screen; Select returns to the selected sub-menu; Exit closes the current screen and moves back up in the hierarchy; Help will display more detailed information on the role of the selected option. The arrow keys allow moving within the list of options and buttons. To exit the configuration program, choose Exit from the main menu. The program then offers to save the changes you've made; accept if you are satisfied with your choices.
			
				Other interfaces have similar features, but they work within more modern graphical interfaces; such as make xconfig which uses a Qt graphical interface, and make gconfig which uses GTK+. The former requires qtbase5-dev, while the latter depends on libglade2-dev and libgtk2.0-dev.
			
				When using one of those configuration interfaces, it is always a good idea to start from a reasonable default configuration. The kernel provides such configurations in arch/arch/configs/*_defconfig and you can put your selected configuration in place with a command like make x86_64_defconfig (in the case of a 64-bit PC) or make i386_defconfig (in the case of a 32-bit PC).
			

TIP Dealing with outdated .config files

				When you provide a .config file that has been generated with another (usually older) kernel version, you will have to update it. You can do so with make oldconfig, it will interactively ask you the questions corresponding to the new configuration options. If you want to use the default answer to all those questions you can use make olddefconfig. With make oldnoconfig, it will assume a negative answer to all questions.
			

### 8.10.4.Â Compiling and Building the Package

NOTE Clean up before rebuilding

				If you have already compiled once in the directory and wish to rebuild everything from scratch (for example, because you substantially changed the kernel configuration), you will have to run make clean to remove the compiled files. make distclean removes even more generated files, including your .config file too, so make sure to backup it first.
			
				Once the kernel configuration is ready, a simple make deb-pkg will generate up to 5 Debian packages:
			linux-image-version
							contains the kernel image and the associated modules,
						linux-headers-version
							contains the header files required to build external modules,
						linux-firmware-image-version
							contains the firmware files needed by some drivers (this package might be missing when you build from the kernel sources provided by Debian),
						linux-image-version-dbg
							contains the debugging symbols for the kernel image and its modules (only created if CONFIG_DEBUG_INFO=y), and
						linux-libc-dev
							contains headers relevant to some user-space libraries like GNU glibc.
						
				The version is defined by the concatenation of the upstream version (as defined by the variables VERSION, PATCHLEVEL, SUBLEVEL, and EXTRAVERSION in the Makefile), of the LOCALVERSION configuration parameter, and of the LOCALVERSION environment variable. The package version reuses the same version string with an appended revision that is regularly incremented (and stored in .version), except if you override it with the KDEB_PKGVERSION environment variable.
			

```bash
$ make deb-pkg LOCALVERSION=-falcot KDEB_PKGVERSION=$(make kernelversion)-1
[...]
$ ls ../*.deb
../linux-headers-5.10.46-falcot_5.10.46-1_amd64.deb
../linux-image-5.10.46-falcot_5.10.46-1_amd64.deb
../linux-image-5.10.46-falcot-dbg_5.10.46-1_amd64.deb
../linux-libc-dev_5.10.46-1_amd64.deb
```

				The whole process requires around 20 GB of free space, at least 8 GB of RAM, and several hours of compilation (utilizing one core) for a standard amd64 Debian kernel. These requirements can be drastically reduced by disabling the debug information using CONFIG_DEBUG_INFO=n, but this will make it impossible to trace kernel errors (âoopsâ) using gdb and also stop the creation of the linux-image-version-dbg package.
			

TIP Missing debian/certs/debian-uefi-certs.pem

				The default kernel in Debian contains the following configuration:
			

```bash
[..]
#
# Certificates for signature checking
#
CONFIG_MODULE_SIG_KEY=""
CONFIG_SYSTEM_TRUSTED_KEYRING=y
CONFIG_SYSTEM_TRUSTED_KEYS="debian/certs/debian-uefi-certs.pem"
# CONFIG_SYSTEM_EXTRA_CERTIFICATE is not set
CONFIG_SECONDARY_TRUSTED_KEYRING=y
CONFIG_SYSTEM_BLACKLIST_KEYRING=y
CONFIG_SYSTEM_BLACKLIST_HASH_LIST=""
# end of Certificates for signature checking
[..]
```

				Building a custom kernel based on these settings will fail if debian/certs/debian-uefi-certs.pem does not exist. This file can either be obtained from the kernel team's Git repository and placed into the source tree, or it can be replaced by your own certificate(s), or you'll have to disable the setting via CONFIG_SYSTEM_TRUSTED_KEYS="". âÂ [https://salsa.debian.org/kernel-team/linux/-/blob/master/debian/certs/debian-uefi-certs.pem](https://salsa.debian.org/kernel-team/linux/-/blob/master/debian/certs/debian-uefi-certs.pem)

### 8.10.5.Â Compiling External Modules

				Some modules are maintained outside of the official Linux kernel. To use them, they must be compiled alongside the matching kernel. A number of common third party modules are provided by Debian in dedicated packages, such as vpb-driver-source (extra modules for Voicetronix telephony hardware) or leds-alix-source (driver of PCEngines ALIX 2/3 boards).
			
				These packages are many and varied, apt-cache rdepends module-assistant$ can show the list provided by Debian. However, a complete list isn't particularly useful since there is no particular reason for compiling external modules except when you know you need it. In such cases, the device's documentation will typically detail the specific module(s) it needs to function under Linux.
			
				For example, let's look at the dahdi-source package: after installation, a .tar.bz2 of the module's sources is stored in /usr/src/. While we could manually extract the tarball and build the module, in practice we prefer to automate all this using the DKMS framework (Dynamic Kernel Module Support). Most modules offer the required DKMS integration in a package ending with a -dkms suffix. In our case, installing dahdi-dkms is all that is needed to compile the kernel module for the current kernel provided that we have the linux-headers-* package matching the installed kernel. For instance, if you use linux-image-amd64, you would also install linux-headers-amd64.
			

QUICK LOOK DKMS and Secure Boot

				On UEFI systems with Secure Boot enabled, modules require signing before they can be loaded. The modules shipped with the kernel packages are signed. But external modules are not. Luckily, dkms already has everything that is required to sign built modules.
			
				First, create a private key /root/dkms.key and a matching certificate /root/dkms.der with openssl, and enroll it with the mokutil --import /root/dkms.der command (please read /usr/share/doc/dkms/README.md.gz for details). This will require a restart and actively importing the certificate using the EFI MOK Manager interface. After this has been done, the sign_tool line in /etc/dkms/framework.conf must be uncommented. Already built modules need to be rebuilt, to be signed by the newly created and enrolled certificate. New modules will be automatically signed.
			

```bash
$ sudo apt install dahdi-dkms
[...]
Setting up dkms (2.8.4-3) ...
Setting up linux-headers-5.10.0-8-amd64 (5.10.46-4) ...
/etc/kernel/header_postinst.d/dkms:
dkms: running auto installation service for kernel 5.10.0-8-amd64:.
Setting up dahdi-dkms (1:2.11.1.0.20170917~dfsg-7.4) ...
Loading new dahdi-2.11.1.0.20170917~dfsg-7.4 DKMS files...
Building for 5.10.0-8-amd64
Building initial module for 5.10.0-8-amd64
Done.

dahdi_dummy.ko:
Running module version sanity check.
 - Original module
   - No original module exists within this kernel
 - Installation
   - Installing to /lib/modules/5.10.0-8-amd64/updates/dkms/

dahdi_dynamic_eth.ko:
Running module version sanity check.
 - Original module
   - No original module exists within this kernel
 - Installation
   - Installing to /lib/modules/5.10.0-8-amd64/updates/dkms/

[...]
DKMS: install completed.
$ sudo dkms status
dahdi, 2.11.1.0.20170917~dfsg-7.4, 5.10.0-8-amd64, x86_64: installed
$ sudo modinfo dahdi_dummy
filename:       /lib/modules/5.10.0-8-amd64/updates/dkms/dahdi_dummy.ko
license:        GPL v2
author:         Robert Pleh <robert.pleh@hermes.si>
description:    Timing-Only Driver
depends:        dahdi
retpoline:      Y
name:           dahdi_dummy
vermagic:       5.10.0-8-amd64 SMP mod_unload modversions 
parm:           debug:int
```

ALTERNATIVE module-assistant

				Before DKMS, module-assistant was the simplest solution to build and deploy kernel modules. It can still be used, in particular for packages lacking DKMS integration: with a simple command like module-assistant auto-install dadhi (or m-a a-i dahdi for short), the modules are compiled for the current kernel, put in a new Debian package, and that package gets installed on the fly.
			

### 8.10.6.Â Applying a Kernel Patch

				Some features are not included in the standard kernel due to a lack of maturity or to some disagreement with the kernel maintainers. Such features may be distributed as patches that anyone is then free to apply to the kernel sources.
			
				Debian sometimes provides some of these patches in linux-patch-* packages, but they often don't make it into stable releases (sometimes for the very same reasons that they are not merged into the official upstream kernel). These packages install files in the /usr/src/kernel-patches/ directory.
			
				To apply one or more of these installed patches, use the patch command in the sources directory then start compilation of the kernel as described above. The following shows an old example using linux-patch-grsecurity2 and linux-source-4.9.
			

```bash
$ cd ~/kernel/linux-source-4.9
$ make clean
$ zcat /usr/src/kernel-patches/diffs/grsecurity2/grsecurity-3.1-4.9.11-201702181444.patch.gz | patch -p1
```

				Note that a given patch may not necessarily work with every version of the kernel; it is possible for patch to fail when applying them to kernel sources. An error message will be displayed and give some details about the failure; in this case, refer to the documentation available in the Debian package of the patch (in the /usr/share/doc/linux-patch-*/ directory). In most cases, the maintainer indicates for which kernel versions their patch is intended.
			[Prev8.9.Â Other Configurations: Time Synchronization, ...](sect.config-misc.en.md)[Up](#)[Home](index.en.md)[Next8.11.Â Installing a Kernel](sect.kernel-installation.en.md)