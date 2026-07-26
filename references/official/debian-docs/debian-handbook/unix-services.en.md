---
title: "9.1.2. The SystemÂ V init system"
source: "https://www.debian.org/doc/manuals/debian-handbook/unix-services.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/unix-services.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.1.2. The SystemÂ V init system"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.kernel-installation.en.md)The Debian Administrator's Handbook[Next](sect.remote-login.en.md)

# ChapterÂ 9.Â Unix Services

[9.1. System Boot](unix-services.en.md#sect.system-boot)[9.1.1. The systemd init system](unix-services.en.md#sect.systemd)[9.1.2. The SystemÂ V init system](unix-services.en.md#sect.sysvinit)[9.2. Remote Login](sect.remote-login.en.md)[9.2.1. Secure Remote Login: SSH](sect.remote-login.en.md#sect.ssh)[9.2.2. Using Remote Graphical Desktops](sect.remote-login.en.md#sect.remote-desktops)[9.3. Managing Rights](sect.rights-management.en.md)[9.3.1. Owners and Permissions](sect.rights-management.en.md#sect.rights-management.perm)[9.3.2. ACLs - Access Control Lists](sect.rights-management.en.md#sect.rights-management.acl)[9.4. Administration Interfaces](sect.administration-interfaces.en.md)[9.4.1. Administrating on a Web Interface: webmin](sect.administration-interfaces.en.md#sect.webmin)[9.4.2. Configuring Packages: debconf](sect.administration-interfaces.en.md#sect.debconf)[9.5. syslog System Events](sect.syslog.en.md)[9.5.1. Principle and Mechanism](sect.syslog.en.md#sect.syslog-principe)[9.5.2. The Configuration File](sect.syslog.en.md#sect.syslog-config)[9.6. The inetd Super-Server](sect.inetd.en.md)[9.7. Scheduling Tasks with cron and atd](sect.task-scheduling-cron-atd.en.md)[9.7.1. Format of a crontab File](sect.task-scheduling-cron-atd.en.md#sect.format-crontab)[9.7.2. Using the at Command](sect.task-scheduling-cron-atd.en.md#sect.at-command)[9.8. Scheduling Asynchronous Tasks: anacron](sect.asynchronous-task-scheduling-anacron.en.md)[9.9. Quotas](sect.quotas.en.md)[9.10. Backup](sect.backup.en.md)[9.10.1. Backing Up with rsync](sect.backup.en.md#id-1.12.13.18)[9.10.2. Restoring Machines without Backups](sect.backup.en.md#id-1.12.13.19)[9.11. Hot Plugging: hotplug](sect.hotplug.en.md)[9.11.1. Introduction](sect.hotplug.en.md#id-1.12.14.2)[9.11.2. The Naming Problem](sect.hotplug.en.md#id-1.12.14.3)[9.11.3. How udev Works](sect.hotplug.en.md#sect.udev)[9.11.4. A concrete example](sect.hotplug.en.md#id-1.12.14.5)[9.12. Power Management: Advanced Configuration and Power Interface (ACPI)](sect.power-management.en.md)
		This chapter covers a number of basic services that are common to many Unix systems. All administrators should be familiar with them.
	

## 9.1.Â System Boot

			When you boot the computer, the many messages scrolling by on the console display many automatic initializations and configurations that are being executed. Sometimes you may wish to slightly alter how this stage works, which means that you need to understand it well. That is the purpose of this section.
		
			On systems with a BIOS, first, the BIOS takes control of the computer, initializes the controllers and hardware, detects the disks, and bridges everything together. Then it looks up the Master Boot Record (MBR) of the first disk in the boot order and loads the code stored there (first stage). This code then launches the second stage and finally executes the bootloader.
		
			In contrast to the BIOS, UEFI is more sophisticated, it knows filesystems and can read the partition tables. The interface searches the system storage for a partition labeled with a specific globally unique identifier (GUID) that marks it as the EFI System Partition (ESP), where the bootloaders, boot managers, UEFI shell, etc., are located, and launches the desired bootloader. If Secure Boot is enabled, the boot process will verify authenticity of the EFI binaries there by signature (thus grub-efi-arch-signed is required in this case). The UEFI specification also defines support for booting in legacy BIOS mode. This is called the Compatibility Support Module (CSM). If CSM is enabled, it will attempt to boot from a drive's MBR. However, many new systems do no longer support the CSM mode.
		
			In both cases then the actual bootloader takes over, finds either a chained bootloader or the kernel on the disk, loads, and executes it. The kernel is then initialized, and starts to search for and mount the partition containing the root filesystem, and finally executes the first program â init. Frequently, this âroot partitionâ and this init are, in fact, located in a virtual filesystem that only exists in RAM (hence its name, âinitramfsâ, formerly called âinitrdâ for âinitialization RAM diskâ). This filesystem is loaded in memory by the bootloader, often from a file on a hard drive or from the network. It contains the bare minimum required by the kernel to load the âtrueâ root filesystem: this may be driver modules for the hard drive, or other devices without which the system cannot boot, or, more frequently, initialization scripts and modules for assembling RAID arrays, opening encrypted partitions, activating LVM volumes, etc. Once the root partition is mounted, the initramfs hands over control to the real init, and the machine goes back to the standard boot process.
		

### 9.1.1.Â The systemd init system

				The âreal initâ is currently provided by systemd and this section documents this init system.
			

CULTURE Before systemd

systemd is a relatively recent âinit systemâ, and although it was already available, to a certain extent, in Wheezy, it has only become the default in Debian Jessie. Previous releases relied, by default, on the âSystemÂ V initâ (in the sysv-rc package), a much more traditional system. We describe the SystemÂ V init later on.
			

ALTERNATIVE Other boot systems

				This book describes the boot system used by default in Debian since Buster (as implemented by the systemd package), as well as the previous default, sysvinit, which is derived and inherited from SystemÂ V Unix systems; there are others.
			
				The upstart system is still not perfectly tested on Debian. It is event based: init scripts are no longer executed in a sequential order but in response to events such as the completion of another script upon which they are dependent. This system, started by Ubuntu, was present in Debian Jessie, but was not the default; it came, in fact, as a replacement for sysvinit, and one of the tasks launched by upstart was to launch the scripts written for traditional systems, especially those from the sysv-rc package.
			
openrc is a dependency based service manager. It was originally written for the Gentoo project, but it aims at being platform agnostic. It maintains compatibility with the System V init system and provides support for booting, changing runlevels, starting and stopping services (in parallel), and shutting down.
			
				There are also other systems and other operating modes, such as file-rc, runit, or minit, but some of them are relatively specialized and not widespread.
			

FigureÂ 9.1.Â Boot sequence of a computer running Linux with systemd

SPECIFIC CASE Booting from the network

				In some configurations, the system may be configured not to execute code from the physical hardware, but to seek its equivalent on the network, making it possible to build computers without a hard drive, or which are completely reinstalled on each boot. This option is not available on all hardware and it generally requires an appropriate combination of firmware and network card.
			
				Booting from the network can be used to launch the debian-installer or FAI (see [SectionÂ 4.1, âInstallation Methodsâ](installation.en.md#sect.installation-methods)).
			

BACK TO BASICS The process, a program instance

				A process is the representation of a running program in memory. It includes all of the information necessary for the proper execution of the software (the code itself, but also the data that it has in memory, the list of files that it has opened, the network connections it has established, etc.). A single program may be instantiated into several processes, not necessarily running under different user IDs.
			

SECURITY Using a shell as init to gain root rights

				By convention, the first process that is booted is the init program (which is a symbolic link to /lib/systemd/systemd by default). However, it is possible to pass an init option to the kernel indicating a different program.
			
				Any person who is able to access the computer can press the Reset button, and thus reboot it. Then, at the bootloader's prompt, it is possible to pass the init=/bin/sh option to the kernel to gain root access without knowing the administrator's password.
			
				To prevent this, you can protect the bootloader itself with a password. You might also think about protecting access to the BIOS (a password protection mechanism is almost always available), without which a malicious intruder could still boot the machine on a removable media containing its own Linux system, which they could then use to access data on the computer's hard drives.
			
				Finally, be aware that most BIOS/EFI implementations have a generic password available. Initially intended for troubleshooting for those who have forgotten their password, these passwords are now public and available on the Internet (see for yourself by searching for âgeneric BIOS passwordsâ in a search engine). All of these protections will thus impede unauthorized access to the machine without being able to completely prevent it. There is no reliable way to protect a computer if the attacker can physically access it; they could dismount the hard drives to connect them to a computer under their own control anyway, or even steal the entire machine, or erase the BIOS memory to reset the passwordâ¦
			
				Systemd executes several processes, in charge of setting up the system: keyboard, drivers, filesystems, network, services. It does this while keeping a global view of the system as a whole, and the requirements of the components. Each component is described by a âunit fileâ (sometimes more); the general syntax is derived from the widely-used â*.ini filesâ syntax, with key = value pairs grouped between [section] headers. Unit files are stored under /lib/systemd/system/ and /etc/systemd/system/; they come in several flavors, but we will focus on âservicesâ and âtargetsâ here.
			
				A systemd â.service fileâ describes a process managed by systemd. It contains roughly the same information as old-style init-scripts, but expressed in a declaratory (and much more concise) way. Systemd handles the bulk of the repetitive tasks (starting and stopping the process, checking its status, logging, dropping privileges, and so on), and the service file only needs to fill in the specifics of the process. For instance, here is the service file for SSH:
			

```bash
[Unit]
Description=OpenBSD Secure Shell server
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target auditd.service
ConditionPathExists=!/etc/ssh/sshd_not_to_be_run

[Service]
EnvironmentFile=-/etc/default/ssh
ExecStartPre=/usr/sbin/sshd -t
ExecStart=/usr/sbin/sshd -D $SSHD_OPTS
ExecReload=/usr/sbin/sshd -t
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartPreventExitStatus=255
Type=notify
RuntimeDirectory=sshd
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
Alias=sshd.service
```

				The [Unit] section contains generic information about the service, like its description and manual page resources, as well as relations (dependency and order) to other services. The [Service] part contains the declarations related to the service execution (starting, stopping, killing, restarting), directories and configuration file(s) used. The last section, [Install], again carries generic information into which targets to install the service and, in this case, the alias that can be used instead of the service name. As you can see, there is very little code in there, only declarations. Systemd takes care of displaying progress reports, keeping track of the processes, and even restarting them when needed. The syntax of these files is fully described in several manual pages (e.g. systemd.service(5), systemd.unit(5), systemd.exec(5), etc.).
			
				A systemd â.target fileâ describes a state of the system where a set of services are known to be operational. It can be thought of as an equivalent of the old-style runlevel. One of the pre-defined targets is local-fs.target; when it is reached, the rest of the system can assume that all local filesystems are mounted and accessible. Other targets include network-online.target and sound.target (for a full list of special targets see systemd.special(7)). The dependencies of a target can be listed either within the target file (in the Requires= line), or using a symbolic link to a service file in the /lib/systemd/system/targetname.target.wants/ directory. For instance, /etc/systemd/system/printer.target.wants/ contains a link to /lib/systemd/system/cups.service; systemd will therefore ensure CUPS is running in order to reach printer.target.
			
				Since unit files are declarative rather than scripts or programs, they cannot be run directly, and they are only interpreted by systemd; several utilities therefore allow the administrator to interact with systemd and control the state of the system and of each component.
			
				The first such utility is systemctl. When run without any arguments, it lists all the unit files known to systemd (except those that have been disabled), as well as their status. systemctl status gives a better view of the services, as well as the related processes. If given the name of a service (as in systemctl status ntp.service), it returns even more details, as well as the last few log lines related to the service (more on that later).
			
				Starting a service by hand is a simple matter of running systemctl start servicename.service. As one can guess, stopping the service is done with systemctl stop servicename.service; other subcommands include reload and restart.
			
				To control whether a service is active (i.e. whether it will get started automatically on boot), use systemctl enable servicename.service (or disable). is-enabled allows checking the status of the service.
			
				An interesting feature of systemd is that it includes a logging component named journald. It comes as a complement to more traditional logging systems such as syslogd, but it adds interesting features such as a formal link between a service and the messages it generates, and the ability to capture error messages generated by its initialization sequence. The messages can be displayed later on, with a little help from the journalctl command. Without any arguments, it simply spews all log messages that occurred since system boot; it will rarely be used in such a manner. Most of the time, it will be used with a service identifier:
			

```bash
# journalctl -u ssh.service
-- Logs begin at Tue 2015-03-31 10:08:49 CEST, end at Tue 2015-03-31 17:06:02 CEST. --
Mar 31 10:08:55 mirtuel sshd[430]: Server listening on 0.0.0.0 port 22.
Mar 31 10:08:55 mirtuel sshd[430]: Server listening on :: port 22.
Mar 31 10:09:00 mirtuel sshd[430]: Received SIGHUP; restarting.
Mar 31 10:09:00 mirtuel sshd[430]: Server listening on 0.0.0.0 port 22.
Mar 31 10:09:00 mirtuel sshd[430]: Server listening on :: port 22.
Mar 31 10:09:32 mirtuel sshd[1151]: Accepted password for roland from 192.168.1.129 port 53394 ssh2
Mar 31 10:09:32 mirtuel sshd[1151]: pam_unix(sshd:session): session opened for user roland by (uid=0)
```

				Another useful command-line flag is -f, which instructs journalctl to keep displaying new messages as they are emitted (much in the manner of tail -f file).
			
				If a service doesn't seem to be working as expected, the first step to solve the problem is to check that the service is actually running with systemctl status; if it is not, and the messages given by the first command are not enough to diagnose the problem, check the logs gathered by journald about that service. For instance, assume the SSH server doesn't work:
			

```bash
# systemctl status ssh.service
â ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled)
   Active: failed (Result: start-limit) since Tue 2015-03-31 17:30:36 CEST; 1s ago
  Process: 1023 ExecReload=/bin/kill -HUP $MAINPID (code=exited, status=0/SUCCESS)
  Process: 1188 ExecStart=/usr/sbin/sshd -D $SSHD_OPTS (code=exited, status=255)
 Main PID: 1188 (code=exited, status=255)

Mar 31 17:30:36 mirtuel systemd[1]: ssh.service: main process exited, code=exited, status=255/n/a
Mar 31 17:30:36 mirtuel systemd[1]: Unit ssh.service entered failed state.
Mar 31 17:30:36 mirtuel systemd[1]: ssh.service start request repeated too quickly, refusing to start.
Mar 31 17:30:36 mirtuel systemd[1]: Failed to start OpenBSD Secure Shell server.
Mar 31 17:30:36 mirtuel systemd[1]: Unit ssh.service entered failed state.
# journalctl -u ssh.service
-- Logs begin at Tue 2015-03-31 17:29:27 CEST, end at Tue 2015-03-31 17:30:36 CEST. --
Mar 31 17:29:27 mirtuel sshd[424]: Server listening on 0.0.0.0 port 22.
Mar 31 17:29:27 mirtuel sshd[424]: Server listening on :: port 22.
Mar 31 17:29:29 mirtuel sshd[424]: Received SIGHUP; restarting.
Mar 31 17:29:29 mirtuel sshd[424]: Server listening on 0.0.0.0 port 22.
Mar 31 17:29:29 mirtuel sshd[424]: Server listening on :: port 22.
Mar 31 17:30:10 mirtuel sshd[1147]: Accepted password for roland from 192.168.1.129 port 38742 ssh2
Mar 31 17:30:10 mirtuel sshd[1147]: pam_unix(sshd:session): session opened for user roland by (uid=0)
Mar 31 17:30:35 mirtuel sshd[1180]: /etc/ssh/sshd_config line 28: unsupported option "yess".
Mar 31 17:30:35 mirtuel systemd[1]: ssh.service: main process exited, code=exited, status=255/n/a
Mar 31 17:30:35 mirtuel systemd[1]: Unit ssh.service entered failed state.
Mar 31 17:30:35 mirtuel sshd[1182]: /etc/ssh/sshd_config line 28: unsupported option "yess".
Mar 31 17:30:35 mirtuel systemd[1]: ssh.service: main process exited, code=exited, status=255/n/a
Mar 31 17:30:35 mirtuel systemd[1]: Unit ssh.service entered failed state.
Mar 31 17:30:35 mirtuel sshd[1184]: /etc/ssh/sshd_config line 28: unsupported option "yess".
Mar 31 17:30:35 mirtuel systemd[1]: ssh.service: main process exited, code=exited, status=255/n/a
Mar 31 17:30:35 mirtuel systemd[1]: Unit ssh.service entered failed state.
Mar 31 17:30:36 mirtuel sshd[1186]: /etc/ssh/sshd_config line 28: unsupported option "yess".
Mar 31 17:30:36 mirtuel systemd[1]: ssh.service: main process exited, code=exited, status=255/n/a
Mar 31 17:30:36 mirtuel systemd[1]: Unit ssh.service entered failed state.
Mar 31 17:30:36 mirtuel sshd[1188]: /etc/ssh/sshd_config line 28: unsupported option "yess".
Mar 31 17:30:36 mirtuel systemd[1]: ssh.service: main process exited, code=exited, status=255/n/a
Mar 31 17:30:36 mirtuel systemd[1]: Unit ssh.service entered failed state.
Mar 31 17:30:36 mirtuel systemd[1]: ssh.service start request repeated too quickly, refusing to start.
Mar 31 17:30:36 mirtuel systemd[1]: Failed to start OpenBSD Secure Shell server.
Mar 31 17:30:36 mirtuel systemd[1]: Unit ssh.service entered failed state.
# vi /etc/ssh/sshd_config
# systemctl start ssh.service
# systemctl status ssh.service
â ssh.service - OpenBSD Secure Shell server
   Loaded: loaded (/lib/systemd/system/ssh.service; enabled)
   Active: active (running) since Tue 2015-03-31 17:31:09 CEST; 2s ago
  Process: 1023 ExecReload=/bin/kill -HUP $MAINPID (code=exited, status=0/SUCCESS)
 Main PID: 1222 (sshd)
   CGroup: /system.slice/ssh.service
           ââ1222 /usr/sbin/sshd -D
#
```

				After checking the status of the service (failed), we went on to check the logs; they indicate an error in the configuration file. After editing the configuration file and fixing the error, we restart the service, then verify that it is indeed running.
			

GOING FURTHER Other types of unit files

				We have only described the most basic of systemd's capabilities in this section. It offers many other interesting features; we will only list a few here:
			
						socket activation: a âsocketâ unit file can be used to describe a network or Unix socket managed by systemd; this means that the socket will be created by systemd, and the actual service may be started on demand when an actual connection attempt comes. This roughly replicates the feature set of inetd. See systemd.socket(5).
					
						timers: a âtimerâ unit file describes events that occur with a fixed frequency or on specific times; when a service is linked to such a timer, the corresponding task will be executed whenever the timer fires. This allows replicating part of the cron features. See systemd.timer(5).
					
						network: a ânetworkâ unit file describes a network interface, which allows configuring such interfaces as well as expressing that a service depends on one particular interface being up. See systemd.network(5).
					

### 9.1.2.Â The SystemÂ V init system

				The SystemÂ V init system (which we'll call init for brevity) executes several processes, following instructions from the /etc/inittab file. The first program that is executed (which corresponds to the sysinit step) is /etc/init.d/rcS, a script that executes all of the programs in the /etc/rcS.d/ directory.
			
				Among these, you will find successively programs in charge of:
			
						configuring the console's keyboard;
					
						loading drivers: most of the kernel modules are loaded by the kernel itself as the hardware is detected; extra drivers are then loaded automatically when the corresponding modules are listed in /etc/modules;
					
						checking the integrity of filesystems;
					
						mounting local partitions;
					
						configuring the network;
					
						mounting network filesystems (NFS).
					

BACK TO BASICS Kernel modules and options

				You have already learned that kernel modules can be loaded during the start of the system by adding them to /etc/modules or a file below /etc/modules-load.d/. But modules can also have options that can be configured by putting some files in /etc/modprobe.d/. These options are defined with directives like this: options module-name option-name=option-value. Several options can be specified with a single directive if necessary.
			
				These configuration files are intended for modprobe â the program that loads a kernel module with its dependencies (modules can indeed call other modules). To list all options of a module use the modinfo -p module command. Both programs are provided by the kmod package together with other tools to handle modules.
			
				Having learned about systemd, services and targets can use the modprobe@module.service to load kernel modules they depend on.
			
				After this stage, init takes over and starts the programs enabled in the default runlevel (which is usually runlevel 2). It executes /etc/init.d/rc 2, a script that starts all services which are listed in /etc/rc2.d/ and whose names start with the âSâ letter. The two-figures number that follows had historically been used to define the order in which services had to be started, but nowadays the default boot system uses insserv, which schedules everything automatically based on the scripts' dependencies. Each boot script thus declares the conditions that must be met to start or stop the service (for example, if it must start before or after another service); init then launches them in the order that meets these conditions. The static numbering of scripts is therefore no longer taken into consideration (but they must always have a name beginning with âSâ followed by two digits and the actual name of the script used for the dependencies). Generally, base services (such as logging with rsyslog, or port assignment with portmap) are started first, followed by standard services and the graphical interface (gdm3).
			
				This dependency-based boot system makes it possible to automate re-numbering, which could be rather tedious if it had to be done manually, and it limits the risks of human error, since scheduling is conducted according to the parameters that are indicated. Another benefit is that services can be started in parallel when they are independent from one another, which can accelerate the boot process.
			
init distinguishes several runlevels, so it can switch from one to another with the telinit new-level command. Immediately, init executes /etc/init.d/rc again with the new runlevel. This script will then start the missing services and stop those that are no longer desired. To do this, it refers to the content of the /etc/rcX.d (where X represents the new runlevel). Scripts starting with âSâ (as in âStartâ) are services to be started; those starting with âKâ (as in âKillâ) are the services to be stopped. The script does not start any service that was already active in the previous runlevel.
			
				By default, SystemÂ V init in Debian uses four different runlevels:
			
						Level 0 is only used temporarily, while the computer is powering down. As such, it only contains many âKâ scripts.
					
						Level 1, also known as single-user mode, corresponds to the system in degraded mode; it includes only basic services, and is intended for maintenance operations where interactions with ordinary users are not desired.
					
						Level 2 is the level for normal operation, which includes networking services, a graphical interface, user logins, etc.
					
						Level 6 is similar to level 0, except that it is used during the shutdown phase that precedes a reboot.
					
				Other levels exist, especially 3 to 5. By default they are configured to operate the same way as level 2, but the administrator can modify them (by adding or deleting scripts in the corresponding /etc/rcX.d directories) to adapt them to particular needs.
			

FigureÂ 9.2.Â Boot sequence of a computer running Linux with SystemÂ V init

				All the scripts contained in the various /etc/rcX.d directories are really only symbolic links â created upon package installation by the update-rc.d program â pointing to the actual scripts which are stored in /etc/init.d/. The administrator can fine tune the services available in each runlevel by re-running update-rc.d with adjusted parameters. The update-rc.d(1) manual page describes the syntax in detail. Please note that removing all symbolic links (with the remove parameter) is not a good method to disable a service. Instead you should simply configure it to not start in the desired runlevel (while preserving the corresponding calls to stop it in the event that the service runs in the previous runlevel). Since update-rc.d has a somewhat convoluted interface, you may prefer using rcconf (from the rcconf package) which provides a more user-friendly interface.
			

DEBIAN POLICY Restarting services

				The maintainer scripts for Debian packages will sometimes restart certain services to ensure their availability or get them to take certain options into account. The command that controls a service â service service operation â doesn't take runlevel into consideration, assumes (wrongly) that the service is currently being used, and may thus initiate incorrect operations (starting a service that was deliberately stopped, or stopping a service that is already stopped, etc.). Debian therefore introduced the invoke-rc.d program: this program must be used by maintainer scripts to run services initialization scripts and it will only execute the necessary commands. Note that, contrary to common usage, the .d suffix is used here in a program name, and not in a directory.
			
				Finally, init starts control programs for various virtual consoles (getty). It displays a prompt, waiting for a username, then executes login user to initiate a session.
			

VOCABULARY Console and terminal

				The first computers were usually separated into several, very large parts: the storage enclosure and the central processing unit were separate from the peripheral devices used by the operators to control them. These were part of a separate furniture, the âconsoleâ. This term was retained, but its meaning has changed. It has become more or less synonymous with âterminalâ, being a keyboard and a screen.
			
				With the development of computers, operating systems have offered several virtual consoles to allow for several independent sessions at the same time, even if there is only one keyboard and screen. Most GNU/Linux systems offer six virtual consoles (in text mode), accessible by typing the key combinations Control+Alt+F1 through Control+Alt+F6.
			
				By extension, the terms âconsoleâ and âterminalâ can also refer to a terminal emulator in a graphical session (such as xterm, gnome-terminal or konsole).
			[Prev8.11.Â Installing a Kernel](sect.kernel-installation.en.md)[Up](#)[Home](index.en.md)[Next9.2.Â Remote Login](sect.remote-login.en.md)