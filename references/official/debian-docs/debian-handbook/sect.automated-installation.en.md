---
title: "12.3.3. Simple-CDD: The All-In-One Solution"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.automated-installation.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.automated-installation.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "12.3.3. Simple-CDD: The All-In-One Solution"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.virtualization.en.md)The Debian Administrator's Handbook[Next](sect.monitoring.en.md)

## 12.3.Â Automated Installation

			The Falcot Corp administrators, like many administrators of large IT services, need tools to install (or reinstall) quickly, and automatically if possible, their new machines.
		
			These requirements can be met by a wide range of solutions. On the one hand, generic tools such as SystemImager handle this by creating an image based on a template machine, then deploy that image to the target systems; at the other end of the spectrum, the standard Debian installer can be preseeded with a configuration file giving the answers to the questions asked during the installation process. As a sort of middle ground, a hybrid tool such as FAI (Fully Automatic Installer) installs machines using the packaging system, but it also uses its own infrastructure for tasks that are more specific to massive deployments (such as starting, partitioning, configuration and so on).
		
			Each of these solutions has its pros and cons: SystemImager works independently from any particular packaging system, which allows it to manage large sets of machines using several distinct Linux distributions. It also includes an update system that doesn't require a reinstallation, but this update system can only be reliable if the machines are not modified independently; in other words, the user must not update any software on their own, or install any other software. Similarly, security updates must not be automated, because they have to go through the centralized reference image maintained by SystemImager. This solution also requires the target machines to be homogeneous, otherwise many different images would have to be kept and managed (an amd64 image won't fit on a powerpc machine, and so on).
		
			On the other hand, an automated installation using debian-installer can adapt to the specifics of each machine: the installer will fetch the appropriate kernel and software packages from the relevant repositories, detect available hardware, partition the whole hard disk to take advantage of all the available space, install the corresponding Debian system, and set up an appropriate bootloader. However, the standard installer will only install standard Debian versions, with the base system and a set of pre-selected âtasksâ; this precludes installing a particular system with non-packaged applications. Fulfilling this particular need requires customizing the installerâ¦ Fortunately, the installer is very modular, and there are tools to automate most of the work required for this customization, most importantly simple-cdd (CDD being an acronym for Custom Debian Derivative). Even this solution, however, only handles initial installations; this is usually not a problem since the APT tools allow efficient deployment of updates later on.
		
			We will only give a rough overview of FAI, and skip SystemImager altogether (which is no longer in Debian, but available as a third-party package), in order to focus more intently on debian-installer and simple-cdd, which are more interesting in a Debian-only context.
		

### 12.3.1.Â Fully Automatic Installer (FAI)

Fully Automatic Installer is probably the oldest automated deployment system for Debian, which explains its status as a reference; but its very flexible nature only just compensates for the complexity it involves.
			
				FAI requires a server system to store deployment information and allow target machines to boot from the network. This server requires the fai-server package (or fai-quickstart, which also brings the required elements for a standard configuration).
			
				FAI uses a specific approach for defining the various installable profiles. Instead of simply duplicating a reference installation, FAI is a full-fledged installer, fully configurable via a set of files and scripts stored on the server; the default location /srv/fai/config/ according to /etc/fai/nfsroot.conf is not automatically created, so the administrator needs to create it along with the relevant files. Most of the times, these files will be customized from the example files available in the documentation for the fai-doc package, more particularly the /usr/share/doc/fai-doc/examples/simple/ directory.
			
				Once the profiles are defined, the fai-setup command generates the elements required to start an FAI installation; this mostly means preparing or updating a minimal system (NFS-root) used during installation. An alternative is to generate a dedicated boot CD with fai-cd.
			
				Creating all these configuration files requires some understanding of the way FAI works. A typical installation process is made of the following steps:
			
						fetching a kernel from the network, and booting it;
					
						mounting the root filesystem from NFS;
					
						executing /usr/sbin/fai, which controls the rest of the process (the next steps are therefore initiated by this script);
					
						copying the configuration space from the server into /fai/;
					
						running fai-class. The /fai/class/[0-9][0-9]* scripts are executed in turn, and return names of âclassesâ that apply to the machine being installed; this information will serve as a base for the following steps. This allows for some flexibility in defining the services to be installed and configured.
					
						fetching a number of configuration variables, depending on the relevant classes;
					
						partitioning the disks and formatting the partitions, based on information provided in /fai/disk_config/class;
					
						mounting said partitions;
					
						installing the base system;
					
						preseeding the Debconf database with fai-debconf;
					
						fetching the list of available packages for APT;
					
						installing the packages listed in /fai/package_config/class;
					
						executing the post-configuration scripts, /fai/scripts/class/[0-9][0-9]*;
					
						recording the installation logs, unmounting the partitions, and rebooting.
					

### 12.3.2.Â Preseeding Debian-Installer

				At the end of the day, the best tool to install Debian systems should logically be the official Debian installer. This is why, right from its inception, debian-installer has been designed for automated use, taking advantage of the infrastructure provided by debconf. The latter allows, on the one hand, to reduce the number of questions asked (hidden questions will use the provided default answer), and on the other hand, to provide the default answers separately, so that installation can be non-interactive. This last feature is known as preseeding.
			

GOING FURTHER Debconf with a centralized database

				Preseeding allows to provide a set of answers to Debconf questions at installation time, but these answers are static and do not evolve as time passes. Since already-installed machines may need upgrading, and new answers may become required, the /etc/debconf.conf configuration file can be set up so that Debconf uses external data sources (such as an LDAP directory server, or a remote file accessed via NFS or Samba). Several external data sources can be defined at the same time, and they complement one another. The local database is still used (for read-write access), but the remote databases are usually restricted to reading. The debconf.conf(5) manual page describes all the possibilities in detail (you need the debconf-doc package).
			

#### 12.3.2.1.Â Using a Preseed File

					There are several places where the installer can get a preseeding file:
				
							in the initrd used to start the machine; in this case, preseeding happens at the very beginning of the installation, and all questions can be avoided. The file just needs to be called preseed.cfg and stored in the initrd root.
						
							on the boot media (CD or USB key); preseeding then happens as soon as the media is mounted, which means right after the questions about language and keyboard layout. The preseed/file boot parameter can be used to indicate the location of the preseeding file (for instance, /cdrom/preseed.cfg when the installation is done off a CD-ROM, or /hd-media/preseed.cfg in the USB-key case).
						
							from the network; preseeding then only happens after the network is (automatically) configured; the relevant boot parameter is then preseed/url=http://server/preseed.cfg (HTTPS, FTPS, SFTP, etc. are not supported).
						
					At a glance, including the preseeding file in the initrd looks like the most interesting solution; however, it is rarely used in practice, because generating an installer initrd is rather complex. The other two solutions are much more common, especially since boot parameters provide another way to preseed the answers to the first questions of the installation process. The usual way to save the bother of typing these boot parameters by hand at each installation is to save them into the configuration for isolinux (in the CD-ROM case) or syslinux (USB key).
				

#### 12.3.2.2.Â Creating a Preseed File

					A preseed file is a plain text file, where each line contains the answer to one Debconf question. A line is split across four fields separated by whitespace (spaces or tabs), as in, for instance, d-i mirror/suite string stable:
				
							the first field is the âownerâ of the question; âd-iâ is used for questions relevant to the installer, but it can also be a package name for questions coming from Debian packages;
						
							the second field is an identifier for the question (the template name);
						
							third, the type of question;
						
							the fourth and last field contains the value for the answer. Note that it must be separated from the third field with a single space; if there are more than one, the following space characters are considered part of the value.
						
					The simplest way to write a preseed file is to install a system by hand. Then debconf-get-selections --installer will provide the answers concerning the installer. Answers about other packages can be obtained with debconf-get-selections. However, a cleaner solution is to write the preseed file by hand, starting from an example and the reference documentation: with such an approach, only questions where the default answer needs to be overridden can be preseeded; using the priority=critical boot parameter will instruct Debconf to only ask critical questions, and use the default answer for others.
				
					Pre-setting a value in a preseed file automatically instructs the Debian installer to not ask that question. This happens, because loading the preseed file does not just set the given value(s), but also marks each of the affected dialogs as âseenâ by the user. Thus it is possible to pre-set a question's value and still present the dialog to the user by resetting the âseenâ flag. Beware that order in this case matters and that the value has to be preseeded before setting the dialog to âunseenâ as shown in the following example:
				

```bash
d-i netcfg/hostname string worker
d-i netcfg/hostname seen false
```

DOCUMENTATION Installation guide appendix

					The installation guide, available online, includes detailed documentation on the use of a preseed file in an appendix. It also includes a detailed and commented sample file, which can serve as a base for local customizations. There are also collections of all debconf templates extracted from each component and suite of Debian: âÂ [https://www.debian.org/releases/stable/amd64/apb](https://www.debian.org/releases/stable/amd64/apb) âÂ [https://www.debian.org/releases/stable/example-preseed.txt](https://www.debian.org/releases/stable/example-preseed.txt) âÂ [https://preseed.debian.net/](https://preseed.debian.net/)

					Preseeding an installation is often not as straightforward as one would wish. It sometimes requires to understand how packages process the given values in their scripts. Don't hesitate to ask on the [debian-cd@lists.debian.org](mailto:debian-cd@lists.debian.org) mailing list or in the #debian-cd IRC channel if you require help. Also be aware that some complex setups still cannot be achieved by preseeding.
				

#### 12.3.2.3.Â Creating a Customized Boot Media

					Knowing where to store the preseed file is all very well, but the location isn't everything: one must, one way or another, alter the installation boot media to change the boot parameters and add the preseed file.
				

##### 12.3.2.3.1.Â Booting From the Network

						When a computer is booted from the network, the server sending the initialization elements also defines the boot parameters. Thus, the change needs to be made in the PXE configuration for the boot server; more specifically, in its /tftpboot/pxelinux.cfg/default configuration file. Setting up network boot is a prerequisite; see the Installation Guide for details. âÂ [https://www.debian.org/releases/stable/amd64/ch04s05](https://www.debian.org/releases/stable/amd64/ch04s05)

##### 12.3.2.3.2.Â Preparing a Bootable USB Key

						Once a bootable key has been prepared (see [SectionÂ 4.1.2, âBooting from a USB Keyâ](installation.en.md#sect.install-usb)), a few extra operations are needed. Assuming the key contents are available under /media/usbdisk/, copy the preseed file to /media/usbdisk/preseed.cfg.
					
						If you have been using a hybrid ISO image to create the bootable USB stick, then you have to edit /media/usbdisk/boot/grub/grub.cfg (for the EFI boot screen):
					

ExampleÂ 12.2.Â boot/grub/grub.cfg file and preseeding parameters

```bash
menuentry --hotkey=i 'Install' {
    set background_color=black
    linux    /install.amd/vmlinuz preseed/file=/cdrom/preseed.cfg locale=en_US.UTF-8 keymap=us language=us country=US vga=788 --- quiet 
    initrd   /install.amd/initrd.gz
}
```

						And you have to edit /media/usbdisk/isolinux/isolinux.cfg (for BIOS boot) or one of the files it utilizes - e.g. /media/usbdisk/isolinux/txt.cfg - to add required boot parameters:
					

ExampleÂ 12.3.Â isolinux/txt.cfg file and preseeding parameters

```bash
label install
        menu label ^Install
        kernel [...]
        append preseed/file=/cdrom/preseed.cfg locale=en_US.UTF-8 keymap=us language=us country=US vga=788 initrd=/install.amd/initrd.gz --- quiet
```

						If you have been using the hd-media installer image for a custom USB stick, edit /media/usbdisk/syslinux.cfg and add the required boot parameters as shown in the example below:
					

ExampleÂ 12.4.Â syslinux.cfg file and preseeding parameters

```bash
default vmlinuz
append preseed/file=/hd-media/preseed.cfg locale=en_US.UTF-8 keymap=us language=us country=US vga=788 initrd=initrd.gz  --
```

##### 12.3.2.3.3.Â Creating a CD-ROM Image

						A USB key is a read-write media, so it was easy for us to add a file there and change a few parameters. In the CD-ROM case, the operation is more complex, since we need to regenerate a full ISO image. This task is handled by debian-cd, but this tool is rather awkward to use: it needs a local mirror, and it requires an understanding of all the options provided by /usr/share/debian-cd/CONF.sh; even then, make must be invoked several times. /usr/share/debian-cd/README is therefore a very recommended read.
					
						Having said that, debian-cd always operates in a similar way: an âimageâ directory with the exact contents of the CD-ROM is generated, then converted to an ISO file with a tool such as genisoimage, mkisofs or xorriso. The image directory is finalized after debian-cd's make image-trees step. At that point, we insert the preseed file into the appropriate directory (usually $TDIR/$CODENAME/CD1/, $TDIR and $CODENAME being parameters defined by the CONF.sh configuration file). The CD-ROM uses isolinux as its bootloader, and its configuration file must be adapted from what debian-cd generated, in order to insert the required boot parameters (the specific files are $TDIR/$CODENAME/CD1/isolinux/isolinux.cfg and $TDIR/$CODENAME/CD1/boot/grub/grub.cfg as shown above). Then the ânormalâ process can be resumed, and we can go on to generating the ISO image with make image CD=1 (or make images if several CD-ROMs are generated).
					

### 12.3.3.Â Simple-CDD: The All-In-One Solution

				Simply using a preseed file is not enough to fulfill all the requirements that may appear for large deployments. Even though it is possible to execute a few scripts at the end of the normal installation process, the selection of the set of packages to install is still not quite flexible (basically, only âtasksâ can be selected); more important, this only allows installing official Debian packages, and precludes locally-generated ones.
			
				On the other hand, debian-cd is able to integrate external packages, and debian-installer can be extended by inserting new steps in the installation process. By combining these capabilities, it should be possible to create a customized installer that fulfills our needs; it should even be able to configure some services after unpacking the required packages. Fortunately, this is not a mere hypothesis, since this is exactly what simple-cdd does.
			
				The purpose of this tool is to allow anyone to easily create a distribution derived from Debian, by selecting a subset of the available packages, preconfiguring them with Debconf, adding specific software, and executing custom scripts at the end of the installation process. This matches the âuniversal operating systemâ philosophy, since anyone can adapt it to their own needs.
			

#### 12.3.3.1.Â Creating Profiles

					Simple-CDD defines âprofilesâ that match the FAI âclassesâ concept, and a machine can have several profiles (determined at installation time). A profile is defined by a set of profiles/profile.* files:
				
							the .description file contains a one-line description for the profile;
						
							the .packages file lists packages that will automatically be installed if the profile is selected;
						
							the .downloads file lists packages that will be stored onto the installation media, but not necessarily installed;
						
							the .preseed file contains preseeding information for Debconf questions (for the installer and/or for packages);
						
							the .postinst file contains a script that will be run at the end of the installation process;
						
							lastly, the .conf file allows changing some parameters based on the profiles to be included in an image.
						
					The default profile has a particular role, since it is always selected; it contains the bare minimum required for Simple-CDD to work. The only thing that is usually customized in this profile is the simple-cdd/profiles preseed parameter: this allows avoiding the question, introduced by Simple-CDD, about what profiles to install.
				
					Note also that the commands will need to be invoked from the parent directory of the profiles directory.
				

#### 12.3.3.2.Â Configuring and Using build-simple-cdd

QUICK LOOK Detailed configuration file

					An example of a Simple-CDD configuration file, with most possible parameters, is included in the package (/usr/share/doc/simple-cdd/examples/simple-cdd.conf.detailed). This can be used as a starting point when creating a custom configuration file. Unfortunately not everything is documented there, so some variables are only listed and explained in /usr/lib/python3/dist-packages/simple_cdd/variables.py.
				
					It is further important to familiarize yourself with the variables understood by /usr/share/debian-cd/CONF.sh.
				
					Simple-CDD requires many parameters to operate fully. They will most often be gathered in a configuration file, which build-simple-cdd can be pointed at with the --conf option, but they can also be specified via dedicated parameters given to build-simple-cdd. Here is an overview of how this command behaves, and how its parameters are used:
				
							the profiles parameter lists the profiles that will be included on the generated CD-ROM image;
						
							based on the list of required packages, Simple-CDD downloads the appropriate files from the server mentioned in server, and gathers them into a partial mirror (which will later be given to debian-cd);
						
							the custom packages mentioned in local_packages are also integrated into this local mirror;
						
							debian-cd is then executed (within a default location that can be configured with the debian_cd_dir variable), with the list of packages to integrate;
						
							once debian-cd has prepared its directory, Simple-CDD applies some changes to this directory:
						
									files containing the profiles are added in a simple-cdd subdirectory (that will end up on the CD-ROM);
								
									other files listed in the all_extras parameter are also added;
								
									the boot parameters are adjusted so as to enable the preseeding. Questions concerning language and country can be avoided if the required information is stored in the language and country variables.
								
							debian-cd then generates the final ISO image.
						

#### 12.3.3.3.Â Generating an ISO Image

					Once we have written a configuration file and defined our profiles, the remaining step is to invoke build-simple-cdd --conf simple-cdd.conf. After a few minutes, we get the required image in images/debian-11-amd64-CD-1.iso.
				[Prev12.2.Â Virtualization](sect.virtualization.en.md)[Up](#)[Home](index.en.md)[Next12.4.Â Monitoring](sect.monitoring.en.md)