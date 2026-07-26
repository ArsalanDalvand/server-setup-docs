---
title: "12.2.3. Virtualization with KVM"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.virtualization.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.virtualization.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "12.2.3. Virtualization with KVM"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](advanced-administration.en.md)The Debian Administrator's Handbook[Next](sect.automated-installation.en.md)

## 12.2.Â Virtualization

			Virtualization is one of the most major advances in the recent years of computing. The term covers various abstractions and techniques simulating virtual computers with a variable degree of independence on the actual hardware. One physical server can then host several systems working at the same time and in isolation. Applications are many, and often derive from this isolation: test environments with varying configurations for instance, or separation of hosted services across different virtual machines for security.
		
			There are multiple virtualization solutions, each with its own pros and cons. This book will focus on Xen, LXC, and KVM, but other noteworthy implementations include the following:
		
					QEMU is a software emulator for a full computer; performances are far from the speed one could achieve running natively, but this allows running unmodified or experimental operating systems on the emulated hardware. It also allows emulating a different hardware architecture: for instance, an amd64 system can emulate an arm computer. QEMU is free software. âÂ [https://qemu.org/](https://qemu.org/)

					Bochs is another free virtual machine, but it only emulates the x86 architectures (i386 and amd64).
				
					VMWare is a proprietary virtual machine; being one of the oldest out there, it is also one of the most widely-known. It works on principles similar to QEMU. VMWare proposes advanced features such as âsnapshottingâ a running virtual machine. âÂ [https://vmware.com/](https://vmware.com/)

					VirtualBox is a virtual machine that is mostly free software (some extra components are available under a proprietary license). Unfortunately it is in Debian's âcontribâ section because it includes some precompiled files that cannot be rebuilt without a proprietary compiler and it currently only resides in Debian Unstable as Oracle's policies make it impossible to keep it secure in a Debian stable release (see [#794466](https://bugs.debian.org/794466)). While younger than VMWare and restricted to the i386 and amd64 architectures, it still includes some âsnapshottingâ and other interesting features. âÂ [https://www.virtualbox.org/](https://www.virtualbox.org/)

HARDWARE Virtualization support

			Some computers might not have hardware virtualization support; when they do, it should be enabled in the BIOS.
		
			To know if you have virtualization support enabled, you can check if the relevant flag is enabled with grep. If the following command for your processor returns some text, you already have virtualization support enabled:
		
					For Intel processors you can execute grep vmx /proc/cpuinfo to check for Intel's Virtual Machine Extensions.
				
					For AMD processors you can execute grep svm /proc/cpuinfo to check for AMD's Secure Virtual Machine.
				
			If that is not the case, you can access the BIOS of your system and check for entries like âIntel Virtualization Technologyâ/âIntel VT-xâ or âSVM modeâ (AMD) - usually to be found in the CPU configuration in the Advanced section.
		

### 12.2.1.Â Xen

				Xen  is a âparavirtualizationâ solution. It introduces a thin abstraction layer, called a âhypervisorâ, between the hardware and the upper systems; this acts as a referee that controls access to hardware from the virtual machines. However, it only handles a few of the instructions, the rest is directly executed by the hardware on behalf of the systems. The main advantage is that performances are not degraded, and systems run close to native speed; the drawback is that the kernels of the operating systems one wishes to use on a Xen hypervisor need to be adapted to run on Xen.
			
				Let's spend some time on terms. The hypervisor is the lowest layer, which runs directly on the hardware, even below the kernel. This hypervisor can split the rest of the software across several domains, which can be seen as so many virtual machines. One of these domains (the first one that gets started) is known as dom0, and has a special role, since only this domain can control the hypervisor and the execution of other domains. These other domains are known as domU. In other words, and from a user point of view, the dom0 matches the âhostâ of other virtualization systems, while a domU can be seen as a âguestâ.
			

CULTURE Xen and the various versions of Linux

				Xen was initially developed as a set of patches that lived out of the official tree, and not integrated to the Linux kernel. At the same time, several upcoming virtualization systems (including KVM) required some generic virtualization-related functions to facilitate their integration, and the Linux kernel gained this set of functions (known as the paravirt_ops or pv_ops interface). Since the Xen patches were duplicating some of the functionality of this interface, they couldn't be accepted officially.
			
				Xensource, the company behind Xen, therefore had to port Xen to this new framework, so that the Xen patches could be merged into the official Linux kernel. That meant a lot of code rewrite, and although Xensource soon had a working version based on the paravirt_ops interface, the patches were only progressively merged into the official kernel. The merge was completed in Linux 3.0. âÂ [https://wiki.xenproject.org/wiki/XenParavirtOps](https://wiki.xenproject.org/wiki/XenParavirtOps)

				Since Jessie is based on version 3.16 of the Linux kernel, the standard linux-image-686-pae and linux-image-amd64 packages include the necessary code, and the distribution-specific patching that was required for Squeeze and earlier versions of Debian is no more. âÂ [https://wiki.xenproject.org/wiki/Xen_Kernel_Feature_Matrix](https://wiki.xenproject.org/wiki/Xen_Kernel_Feature_Matrix)

CULTURE Xen and non-Linux kernels

				Xen requires modifications to all the operating systems one wants to run on it; not all kernels have the same level of maturity in this regard. Many are fully-functional, both as dom0 and domU: Linux 3.0 and later, NetBSD 4.0 and later, and OpenSolaris. Others only work as a domU. You can check the status of each operating system in the Xen wiki: âÂ [https://wiki.xenproject.org/wiki/Dom0_Kernels_for_Xen](https://wiki.xenproject.org/wiki/Dom0_Kernels_for_Xen) âÂ [https://wiki.xenproject.org/wiki/DomU_Support_for_Xen](https://wiki.xenproject.org/wiki/DomU_Support_for_Xen)

				However, if Xen can rely on the hardware functions dedicated to virtualization (which are only present in more recent processors), even non-modified operating systems can run as domU (including Windows).
			

NOTE Architectures compatible with Xen

				Xen is currently only available for the i386, amd64, arm64 and armhf architectures.
			
				Using Xen under Debian requires three components:
			
						The hypervisor itself. According to the available hardware, the appropriate package providing xen-hypervisor will be either xen-hypervisor-4.14-amd64, xen-hypervisor-4.14-armhf, or xen-hypervisor-4.14-arm64.
					
						A kernel that runs on that hypervisor. Any kernel more recent than 3.0 will do, including the 5.10 version present in Bullseye.
					
						The i386 architecture also requires a standard library with the appropriate patches taking advantage of Xen; this is in the libc6-xen package.
					
				The hypervisor also brings xen-utils-4.14, which contains tools to control the hypervisor from the dom0. This in turn brings the appropriate standard library. During the installation of all that, configuration scripts also create a new entry in the GRUB bootloader menu, so as to start the chosen kernel in a Xen dom0. Note, however, that this entry is not usually set to be the first one in the list, but it will be selected by default.
			
				Once these prerequisites are installed, the next step is to test the behavior of the dom0 by itself; this involves a reboot to the hypervisor and the Xen kernel. The system should boot in its standard fashion, with a few extra messages on the console during the early initialization steps.
			
				Now is the time to actually install useful systems on the domU systems, using the tools from xen-tools. This package provides the xen-create-image command, which largely automates the task. The only mandatory parameter is --hostname, giving a name to the domU; other options are important, but they can be stored in the /etc/xen-tools/xen-tools.conf configuration file, and their absence from the command line doesn't trigger an error. It is therefore important to either check the contents of this file before creating images, or to use extra parameters in the xen-create-image invocation. Important parameters of note include the following:
			
--memory, to specify the amount of RAM dedicated to the newly created system;
					
--size and --swap, to define the size of the âvirtual disksâ available to the domU;
					
--debootstrap-cmd, to specify the which debootstrap command is used. The default is debootstrap if debootstrap and cdebootstrap are installed. In that case, the --dist option will also most often be used (with a distribution name such as bullseye).
					

GOING FURTHER Installing a non-Debian system in a domU

						In case of a non-Linux system, care should be taken to define the kernel the domU must use, using the --kernel option.
					
--dhcp states that the domU's network configuration should be obtained by DHCP while --ip allows defining a static IP address.
					
						Lastly, a storage method must be chosen for the images to be created (those that will be seen as hard disk drives from the domU). The simplest method, corresponding to the --dir option, is to create one file on the dom0 for each device the domU should be provided. For systems using LVM, the alternative is to use the --lvm option, followed by the name of a volume group; xen-create-image will then create a new logical volume inside that group, and this logical volume will be made available to the domU as a hard disk drive.
					

NOTE Storage in the domU

						Entire hard disks can also be exported to the domU, as well as partitions, RAID arrays or pre-existing LVM logical volumes. These operations are not automated by xen-create-image, however, so editing the Xen image's configuration file is in order after its initial creation with xen-create-image.
					
				Once these choices are made, we can create the image for our future Xen domU:
			

```bash
# xen-create-image --hostname testxen --dhcp --dir /srv/testxen --size=2G --dist=bullseye --role=udev

General Information
--------------------
Hostname       :  testxen
Distribution   :  bullseye
Mirror         :  http://deb.debian.org/debian
Partitions     :  swap            512M  (swap)
                  /               2G    (ext4)
Image type     :  sparse
Memory size    :  256M
Bootloader     :  pygrub

[...]
Logfile produced at:
	 /var/log/xen-tools/testxen.log

Installation Summary
---------------------
Hostname        :  testxen
Distribution    :  bullseye
MAC Address     :  00:16:3E:C2:07:EE
IP Address(es)  :  dynamic
SSH Fingerprint :  SHA256:K+0QjpGzZOacLZ3jX4gBwp0mCESt5ceN5HCJZSKWS1A (DSA)
SSH Fingerprint :  SHA256:9PnovvGRuTw6dUcEVzzPKTITO0+3Ki1Gs7wu4ke+4co (ECDSA)
SSH Fingerprint :  SHA256:X5z84raKBajUkWBQA6MVuanV1OcV2YIeD0NoCLLo90k (ED25519)
SSH Fingerprint :  SHA256:VXu6l4tsrCoRsXOqAwvgt57sMRj2qArEbOzHeydvV34 (RSA)
Root Password   :  FS7CUxsY3xkusv7EkbT9yae
```

				We now have a virtual machine, but it is currently not running (and therefore only using space on the dom0's hard disk). Of course, we can create more images, possibly with different parameters.
			
				Before turning these virtual machines on, we need to define how they'll be accessed. They can of course be considered as isolated machines, only accessed through their system console, but this rarely matches the usage pattern. Most of the time, a domU will be considered as a remote server, and accessed only through a network. However, it would be quite inconvenient to add a network card for each domU; which is why Xen allows creating virtual interfaces that each domain can see and use in a standard way. Note that these cards, even though they're virtual, will only be useful once connected to a network, even a virtual one. Xen has several network models for that:
			
						The simplest model is the bridge model; all the eth0 network cards (both in the dom0 and the domU systems) behave as if they were directly plugged into an Ethernet switch.
					
						Then comes the routing model, where the dom0 behaves as a router that stands between the domU systems and the (physical) external network.
					
						Finally, in the NAT model, the dom0 is again between the domU systems and the rest of the network, but the domU systems are not directly accessible from outside, and traffic goes through some network address translation on the dom0.
					
				These three networking nodes involve a number of interfaces with unusual names, such as vif*, veth*, peth* and xenbr0. The Xen hypervisor arranges them in whichever layout has been defined, under the control of the user-space tools. Since the NAT and routing models are only adapted to particular cases, we will only address the bridging model.
			
				The standard configuration of the Xen packages does not change the system-wide network configuration. However, the xend daemon is configured to integrate virtual network interfaces into any pre-existing network bridge (with xenbr0 taking precedence if several such bridges exist). We must therefore set up a bridge in /etc/network/interfaces (which requires installing the bridge-utils package, which is why the xen-utils package recommends it) to replace the existing eth0 entry (be careful to use the correct network device name):
			

```bash
auto xenbr0
iface xenbr0 inet dhcp
    bridge_ports eth0
    bridge_maxwait 0
```

				After rebooting to make sure the bridge is automatically created, we can now start the domU with the Xen control tools, in particular the xl command. This command allows different manipulations on the domains, including listing them and, starting/stopping them. You might need to increase the default memory by editing the variable memory from configuration file (in this case, /etc/xen/testxen.cfg). Here we have set it to 1024 (megabytes).
			

```bash
# xl list
Name                                        ID   Mem VCPUs	State	Time(s)
Domain-0                                     0  3918     2     r-----      35.1
# xl create /etc/xen/testxen.cfg
Parsing config from /etc/xen/testxen.cfg
# xl list
Name                                        ID   Mem VCPUs	State	Time(s)
Domain-0                                     0  2757     2     r-----      45.2
testxen                                      3  1024     1     r-----       1.3
```

TOOL Choice of toolstacks to manage Xen VM

				In Debian 7 and older releases, xm was the reference command line tool to use to manage Xen virtual machines. It has now been replaced by xl which is mostly backwards compatible. But those are not the only available tools: virsh of libvirt and xe of XenServer's XAPI (commercial offering of Xen) are alternative tools.
			

CAUTION Only one domU per image!

				While it is of course possible to have several domU systems running in parallel, they will all need to use their own image, since each domU is made to believe it runs on its own hardware (apart from the small slice of the kernel that talks to the hypervisor). In particular, it isn't possible for two domU systems running simultaneously to share storage space. If the domU systems are not run at the same time, it is, however, quite possible to reuse a single swap partition, or the partition hosting the /home filesystem.
			
				Note that the testxen domU uses real memory taken from the RAM that would otherwise be available to the dom0, not simulated memory. Care should therefore be taken, when building a server meant to host Xen instances, to provision the physical RAM accordingly.
			
				VoilÃ ! Our virtual machine is starting up. We can access it in one of two modes. The usual way is to connect to it âremotelyâ through the network, as we would connect to a real machine; this will usually require setting up either a DHCP server or some DNS configuration. The other way, which may be the only way if the network configuration was incorrect, is to use the hvc0 console, with the xl console command:
			

```bash
# xl console testxen
[...]

Debian GNU/Linux 11 testxen hvc0

testxen login:
```

				One can then open a session, just like one would do if sitting at the virtual machine's keyboard. Detaching from this console is achieved through the Control+] key combination.
			

TIP Getting the console straight away

				Sometimes one wishes to start a domU system and get to its console straight away; this is why the xl create command takes a -c switch. Starting a domU with this switch will display all the messages as the system boots.
			

TOOL Graphical Xen managers

				OpenXenManager (in the openxenmanager package), a graphical interface allowing remote management of Xen domains via Xen's API, is no longer provided by Debian due to the lack of upstream development. If you are looking for a replacement, virt-manager provides support to handle Xen VMs as well.
			
				Once the domU is up, it can be used just like any other server (since it is a GNU/Linux system after all). However, its virtual machine status allows some extra features. For instance, a domU can be temporarily paused then resumed, with the xl pause and xl unpause commands. Note that even though a paused domU does not use any processor power, its allocated memory is still in use. It may be interesting to consider the xl save and xl restore commands: saving a domU frees the resources that were previously used by this domU, including RAM. When restored (or unpaused, for that matter), a domU doesn't even notice anything beyond the passage of time. If a domU was running when the dom0 is shut down, the packaged scripts automatically save the domU, and restore it on the next boot. This will of course involve the standard inconvenience incurred when hibernating a laptop computer, for instance; in particular, if the domU is suspended for too long, network connections may expire. Note also that Xen is so far incompatible with a large part of ACPI power management, which precludes suspending the host (dom0) system.
			
				Halting or rebooting a domU can be done either from within the domU (with the shutdown command) or from the dom0, with xl shutdown or xl reboot.
			
				Most of the xl subcommands expect one or more arguments, often a domU name. These arguments are well described in the xl(1) manual page.
			

GOING FURTHER Advanced Xen

				Xen has many more features than we can describe in these few paragraphs. In particular, the system is very dynamic, and many parameters for one domain (such as the amount of allocated memory, the visible hard drives, the behavior of the task scheduler, and so on) can be adjusted even when that domain is running. A domU can even be migrated across servers without being shut down, and without losing its network connections! For all these advanced aspects, the primary source of information is the official Xen documentation. âÂ [https://xenproject.org/help/documentation/](https://xenproject.org/help/documentation/)

### 12.2.2.Â LXC

				Even though it is used to build âvirtual machinesâ, LXC is not, strictly speaking, a virtualization system, but a system to isolate groups of processes from each other even though they all run on the same host. It takes advantage of a set of recent evolutions in the Linux kernel, collectively known as control groups, by which different sets of processes called âgroupsâ have different views of certain aspects of the overall system. Most notable among these aspects are the process identifiers, the network configuration, and the mount points. Such a group of isolated processes will not have any access to the other processes in the system, and its accesses to the filesystem can be restricted to a specific subset. It can also have its own network interface and routing table, and it may be configured to only see a subset of the available devices present on the system.
			
				These features can be combined to isolate a whole process family starting from the init process, and the resulting set looks very much like a virtual machine. The official name for such a setup is a âcontainerâ (hence the LXC moniker: LinuX Containers), but a rather important difference with ârealâ virtual machines such as provided by Xen or KVM is that there is no second kernel; the container uses the very same kernel as the host system. This has both pros and cons: advantages include excellent performance due to the total lack of overhead, and the fact that the kernel has a global vision of all the processes running on the system, so the scheduling can be more efficient than it would be if two independent kernels were to schedule different task sets. Chief among the inconveniences is the impossibility to run a different kernel in a container (whether a different Linux version or a different operating system altogether).
			

NOTE LXC isolation limits

				LXC containers do not provide the level of isolation achieved by heavier emulators or virtualizers. In particular:
			
						since the kernel is shared among the host system and the containers, processes constrained to containers can still access the kernel messages, which can lead to information leaks if messages are emitted by a container;
					
						for similar reasons, if a container is compromised and a kernel vulnerability is exploited, the other containers may be affected too;
					
						on the filesystem, the kernel checks permissions according to the numerical identifiers for users and groups; these identifiers may designate different users and groups depending on the container, which should be kept in mind if writable parts of the filesystem are shared among containers.
					
				Since we are dealing with isolation and not plain virtualization, setting up LXC containers is more complex than just running debian-installer on a virtual machine. We will describe a few prerequisites, then go on to the network configuration; we will then be able to actually create the system to be run in the container.
			

#### 12.2.2.1.Â Preliminary Steps

					The lxc package contains the tools required to run LXC, and must therefore be installed.
				
					LXC also requires the control groups configuration system, which is a virtual filesystem to be mounted on /sys/fs/cgroup. Since Debian 8 switched to systemd, which also relies on control groups, this is now done automatically at boot time without further configuration.
				

#### 12.2.2.2.Â Network Configuration

					The goal of installing LXC is to set up virtual machines; while we could, of course, keep them isolated from the network, and only communicate with them via the filesystem, most use cases involve giving at least minimal network access to the containers. In the typical case, each container will get a virtual network interface, connected to the real network through a bridge. This virtual interface can be plugged either directly onto the host's physical network interface (in which case the container is directly on the network), or onto another virtual interface defined on the host (and the host can then filter or route traffic). In both cases, the bridge-utils package will be required.
				
					The simple case is just a matter of editing /etc/network/interfaces, moving the configuration for the physical interface (for instance, eth0 or enp1s0) to a bridge interface (usually br0), and configuring the link between them. For instance, if the network interface configuration file initially contains entries such as the following:
				

```bash
auto eth0
iface eth0 inet dhcp
```

					They should be disabled and replaced with the following:
				

```bash
auto br0
iface br0 inet dhcp
    bridge-ports eth0
```

					The effect of this configuration will be similar to what would be obtained if the containers were machines plugged into the same physical network as the host. The âbridgeâ configuration manages the transit of Ethernet frames between all the bridged interfaces, which includes the physical eth0 as well as the interfaces defined for the containers.
				
					In cases where this configuration cannot be used (for instance, if no public IP addresses can be assigned to the containers), a virtual tap interface will be created and connected to the bridge. The equivalent network topology then becomes that of a host with a second network card plugged into a separate switch, with the containers also plugged into that switch. The host must then act as a gateway for the containers if they are meant to communicate with the outside world.
				
					In addition to bridge-utils, this ârichâ configuration requires the vde2 package; the /etc/network/interfaces file then becomes:
				

```bash
# Interface eth0 is unchanged
auto eth0
iface eth0 inet dhcp

# Virtual interface 
auto tap0
iface tap0 inet manual
    vde2-switch -t tap0

# Bridge for containers
auto br0
iface br0 inet static
    bridge-ports tap0
    address 10.0.0.1
    netmask 255.255.255.0
```

					The network can then be set up either statically in the containers, or dynamically with DHCP server running on the host. Such a DHCP server will need to be configured to answer queries on the br0 interface.
				

#### 12.2.2.3.Â Setting Up the System

					Let us now set up the filesystem to be used by the container. Since this âvirtual machineâ will not run directly on the hardware, some tweaks are required when compared to a standard filesystem, especially as far as the kernel, devices and consoles are concerned. Fortunately, the lxc package includes scripts that mostly automate this configuration. For instance, the following commands (which require the debootstrap and rsync packages) will install a Debian container:
				

```bash
# lxc-create -n testlxc -t debian
debootstrap is /usr/sbin/debootstrap
Checking cache download in /var/cache/lxc/debian/rootfs-stable-amd64 ... 
Downloading debian minimal ...
I: Retrieving Release 
I: Retrieving Release.gpg 
[...]
Download complete.
Copying rootfs to /var/lib/lxc/testlxc/rootfs...
[...]
#
```

					Note that the filesystem is initially created in /var/cache/lxc, then moved to its destination directory. This allows creating identical containers much more quickly, since only copying is then required.
				
					Note that the Debian template creation script accepts an --arch option to specify the architecture of the system to be installed and a --release option if you want to install something else than the current stable release of Debian. You can also set the MIRROR environment variable to point to a local Debian mirror.
				
					The lxc package further creates a bridge interface lxcbr0, which by default is used by all newly created containers via /etc/lxc/default.conf and the lxc-net service:
				

```bash
lxc.net.0.type = veth
lxc.net.0.link = lxcbr0
lxc.net.0.flags = up
```

					These entries mean, respectively, that a virtual interface will be created in every new container; that it will automatically be brought up when said container is started; and that it will be automatically connected to the lxcbr0 bridge on the host. You will find these settings in the created container's configuration (/var/lib/lxc/testlxc/config), where also the device' MAC address will be specified in lxc.net.0.hwaddr. Should this last entry be missing or disabled, a random MAC address will be generated.
				
					Another useful entry in that file is the setting of the hostname:
				

```bash
lxc.uts.name = testlxc
```

					The newly-created filesystem now contains a minimal Debian system and a network interface.
				

#### 12.2.2.4.Â Starting the Container

					Now that our virtual machine image is ready, let's start the container with lxc-start --name=testlxc.
				
					In LXC releases following 2.0.8, root passwords are not set by default. We can set one running lxc-attach -n testlxc passwd if we want. We can login with:
				

```bash
# lxc-console -n testlxc
Connected to tty 1
Type <Ctrl+a q> to exit the console, <Ctrl+a Ctrl+a> to enter Ctrl+a itself

Debian GNU/Linux 11 testlxc tty1

testlxc login: root
Password: 
Linux testlxc 5.10.0-11-amd64 #1 SMP Debian 5.10.92-1 (2022-01-18) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Mar  9 01:45:21 UTC 2022 on console
root@testlxc:~# ps auxwf
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.2  18964 11464 ?        Ss   01:36   0:00 /sbin/init
root          45  0.0  0.2  31940 10396 ?        Ss   01:37   0:00 /lib/systemd/systemd-journald
root          71  0.0  0.1  99800  5724 ?        Ssl  01:37   0:00 /sbin/dhclient -4 -v -i -pf /run/dhclient.eth0.pid [..]
root          97  0.0  0.1  13276  6980 ?        Ss   01:37   0:00 sshd: /usr/sbin/sshd -D [listener] 0 of 10-100 startups
root         160  0.0  0.0   6276  3928 pts/0    Ss   01:46   0:00 /bin/login -p --
root         169  0.0  0.0   7100  3824 pts/0    S    01:51   0:00  \_ -bash
root         172  0.0  0.0   9672  3348 pts/0    R+   01:51   0:00      \_ ps auxwf
root         164  0.0  0.0   5416  2128 pts/1    Ss+  01:49   0:00 /sbin/agetty -o -p -- \u --noclear [...]
root@testlxc:~#
```

					We are now in the container; our access to the processes is restricted to only those started from the container itself, and our access to the filesystem is similarly restricted to the dedicated subset of the full filesystem (/var/lib/lxc/testlxc/rootfs). We can exit the console with Control+a q.
				
					Note that we ran the container as a background process, thanks to lxc-start starting using the --daemon option by default. We can interrupt the container with a command such as lxc-stop --name=testlxc.
				
					The lxc package contains an initialization script that can automatically start one or several containers when the host boots (it relies on lxc-autostart which starts containers whose lxc.start.auto option is set to 1). Finer-grained control of the startup order is possible with lxc.start.order and lxc.group: by default, the initialization script first starts containers which are part of the onboot group and then the containers which are not part of any group. In both cases, the order within a group is defined by the lxc.start.order option.
				

GOING FURTHER Mass virtualization

					Since LXC is a very lightweight isolation system, it can be particularly adapted to massive hosting of virtual servers. The network configuration will probably be a bit more advanced than what we described above, but the ârichâ configuration using tap and veth interfaces should be enough in many cases.
				
					It may also make sense to share part of the filesystem, such as the /usr and /lib subtrees, so as to avoid duplicating the software that may need to be common to several containers. This will usually be achieved with lxc.mount.entry entries in the containers configuration file. An interesting side-effect is that the processes will then use less physical memory, since the kernel is able to detect that the programs are shared. The marginal cost of one extra container can then be reduced to the disk space dedicated to its specific data, and a few extra processes that the kernel must schedule and manage.
				
					We haven't described all the available options, of course; more comprehensive information can be obtained from the lxc(7) and lxc.container.conf(5) manual pages and the ones they reference.
				

### 12.2.3.Â Virtualization with KVM

				KVM, which stands for Kernel-based Virtual Machine, is first and foremost a kernel module providing most of the infrastructure that can be used by a virtualizer, but it is not a virtualizer by itself. Actual control for the virtualization is handled by a QEMU-based application. Don't worry if this section mentions qemu-* commands: it is still about KVM.
			
				Unlike other virtualization systems, KVM was merged into the Linux kernel right from the start. Its developers chose to take advantage of the processor instruction sets dedicated to virtualization (Intel-VT and AMD-V), which keeps KVM lightweight, elegant and not resource-hungry. The counterpart, of course, is that KVM doesn't work on any computer but only on those with appropriate processors. For x86-based computers, you can verify that you have such a processor by looking for âvmxâ or âsvmâ in the CPU flags listed in /proc/cpuinfo.
			
				With RedÂ Hat actively supporting its development, KVM has more or less become the reference for Linux virtualization.
			

#### 12.2.3.1.Â Preliminary Steps

					Unlike such tools as VirtualBox, KVM itself doesn't include any user-interface for creating and managing virtual machines. The virtual qemu-kvm package only provides an executable able to start a virtual machine, as well as an initialization script that loads the appropriate kernel modules.
				
					Fortunately, RedÂ Hat also provides another set of tools to address that problem, by developing the libvirt library and the associated virtual machine manager tools. libvirt allows managing virtual machines in a uniform way, independently of the virtualization system involved behind the scenes (it currently supports QEMU, KVM, Xen, LXC, OpenVZ, VirtualBox, VMWare, and UML). virt-manager is a graphical interface that uses libvirt to create and manage virtual machines.
				
					We first install the required packages, with apt-get install libvirt-clients libvirt-daemon-system qemu-kvm virtinst virt-manager virt-viewer. libvirt-daemon-system provides the libvirtd daemon, which allows (potentially remote) management of the virtual machines running of the host, and starts the required VMs when the host boots. libvirt-clients provides the virsh command-line tool, which allows controlling the libvirtd-managed machines.
				
					The virtinst package provides virt-install, which allows creating virtual machines from the command line. Finally, virt-viewer allows accessing a VM's graphical console.
				

#### 12.2.3.2.Â Network Configuration

					Just as in Xen and LXC, the most frequent network configuration involves a bridge grouping the network interfaces of the virtual machines (see [SectionÂ 12.2.2.2, âNetwork Configurationâ](sect.virtualization.en.md#sect.lxc.network)).
				
					Alternatively, and in the default configuration provided by KVM, the virtual machine is assigned a private address (in the 192.168.122.0/24 range), and NAT is set up so that the VM can access the outside network.
				
					The rest of this section assumes that the host has an eth0 physical interface and a br0 bridge, and that the former is connected to the latter.
				

#### 12.2.3.3.Â Installation with virt-install

					Creating a virtual machine is very similar to installing a normal system, except that the virtual machine's characteristics are described in a seemingly endless command line.
				
					Practically speaking, this means we will use the Debian installer, by booting the virtual machine on a virtual DVD-ROM drive that maps to a Debian DVD image stored on the host system. The VM will export its graphical console over the VNC protocol (see [SectionÂ 9.2.2, âUsing Remote Graphical Desktopsâ](sect.remote-login.en.md#sect.remote-desktops) for details), which will allow us to control the installation process.
				
					We first need to tell libvirtd where to store the disk images, unless the default location (/var/lib/libvirt/images/) is fine.
				

```bash
# mkdir /srv/kvm
# virsh pool-create-as srv-kvm dir --target /srv/kvm
Pool srv-kvm created

#
```

TIP Add your user to the libvirt group

					All samples in this section assume that you are running commands as root. Effectively, if you want to control a local libvirt daemon, you need either to be root or to be a member of the libvirt group (which is not the case by default). Thus if you want to avoid using root rights too often, you can add yourself to the libvirt group and run the various commands under your user identity.
				
					Let us now start the installation process for the virtual machine, and have a closer look at virt-install's most important options. This command registers the virtual machine and its parameters in libvirtd, then starts it so that its installation can proceed.
				

```bash
# virt-install --connect qemu:///system  
               --virt-type kvm           
               --name testkvm            
               --memory 2048             
               --disk /srv/kvm/testkvm.qcow,format=qcow2,size=10  
               --cdrom /srv/isos/debian-11.2.0-amd64-netinst.iso  
               --network bridge=virbr0   
               --graphics vnc            
               --os-type linux           
               --os-variant debiantesting

Starting install...
Allocating 'testkvm.qcow'
```

[](#virtinst.connect)

							The --connect option specifies the âhypervisorâ to use. Its form is that of an URL containing a virtualization system (xen://, qemu://, lxc://, openvz://, vbox://, and so on) and the machine that should host the VM (this can be left empty in the case of the local host). In addition to that, and in the QEMU/KVM case, each user can manage virtual machines working with restricted permissions, and the URL path allows differentiating âsystemâ machines (/system) from others (/session).
						

[](#virtinst.type)

							Since KVM is managed the same way as QEMU, the --virt-type kvm allows specifying the use of KVM even though the URL looks like QEMU.
						

[](#virtinst.name)

							The --name option defines a (unique) name for the virtual machine.
						

[](#virtinst.ram)

							The --memory option allows specifying the amount of RAM (in MB) to allocate for the virtual machine.
						

[](#virtinst.disk)

							The --disk specifies the location of the image file that is to represent our virtual machine's hard disk; that file is created, unless present, with a size (in GB) specified by the size parameter. The format parameter allows choosing among several ways of storing the image file. The default format (qcow2) allows starting with a small file that only grows when the virtual machine starts actually using space.
						

[](#virtinst.cdrom)

							The --cdrom option is used to indicate where to find the optical disk to use for installation. The path can be either a local path for an ISO file, an URL where the file can be obtained, or the device file of a physical CD-ROM drive (i.e. /dev/cdrom).
						

[](#virtinst.network)

							The --network specifies how the virtual network card integrates in the host's network configuration. The default behavior (which we explicitly forced in our example) is to integrate it into any pre-existing network bridge. If no such bridge exists, the virtual machine will only reach the physical network through NAT, so it gets an address in a private subnet range (192.168.122.0/24).
						
							The default network configuration, which contains the definition for a virbr0 bridge interface, can be edited using virsh net-edit default and started via virsh net-start default if not already done automatically during system start.
						

[](#virtinst.vnc)

--graphics vnc states that the graphical console should be made available using VNC. The default behavior for the associated VNC server is to only listen on the local interface; if the VNC client is to be run on a different host, establishing the connection will require setting up an SSH tunnel (see [SectionÂ 9.2.1.4, âCreating Encrypted Tunnels with Port Forwardingâ](sect.remote-login.en.md#sect.ssh-port-forwarding)). Alternatively, --graphics vnc,listen=0.0.0.0 can be used so that the VNC server is accessible from all interfaces; note that if you do that, you really should design your firewall accordingly.
						

[](#virtinst.os)

							The --os-type and --os-variant options allow optimizing a few parameters of the virtual machine, based on some of the known features of the operating system mentioned there.
						
							The full list of OS types can be shown using the osinfo-query os command from the libosinfo-bin package.
						
					At this point, the virtual machine is running, and we need to connect to the graphical console to proceed with the installation process. If the previous operation was run from a graphical desktop environment, this connection should be automatically started. If not, or if we operate remotely, virt-viewer can be run from any graphical environment to open the graphical console (note that the root password of the remote host is asked twice because the operation requires 2 SSH connections):
				

```bash
$ virt-viewer --connect qemu+ssh://root@server/system testkvm
root@server's password: 
root@server's password:
```

FigureÂ 12.1.Â Connecting to installer session using virt-viewer

					When the installation process ends, the virtual machine is restarted, now ready for use.
				

#### 12.2.3.4.Â Managing Machines with virsh

					Now that the installation is done, let us see how to handle the available virtual machines. The first thing to try is to ask libvirtd for the list of the virtual machines it manages:
				

```bash
# virsh -c qemu:///system list --all
 Id Name                 State
----------------------------------
  8 testkvm              shut off
```

					Let's start our test virtual machine:
				

```bash
# virsh -c qemu:///system start testkvm
Domain testkvm started
```

					We can now get the connection instructions for the graphical console (the returned VNC display can be given as parameter to vncviewer):
				

```bash
# virsh -c qemu:///system vncdisplay testkvm
127.0.0.1:0
```

					Other available virsh subcommands include:
				
reboot to restart a virtual machine;
						
shutdown to trigger a clean shutdown;
						
destroy, to stop it brutally;
						
suspend to pause it;
						
resume to unpause it;
						
autostart to enable (or disable, with the --disable option) starting the virtual machine automatically when the host starts;
						
undefine to remove all traces of the virtual machine from libvirtd.
						
					All these subcommands take a virtual machine identifier as a parameter.
				

Installing an RPM based chroot in Debian with yum

					If a chroot is meant to run Debian (or one of its derivatives), the system can be initialized with debootstrap. But if it is to be installed with an RPM-based system (such as Fedora, CentOS or Scientific Linux), the setup will need to be done using the yum utility, available as yum4 in the nextgen-yum4 package, since the original program has been removed from Debian before the Bullseye release due to being unmaintained, outdated, and obsoleted by dnf.
				
					The procedure requires using rpm to extract an initial set of files, including notably yum configuration files, and then calling yum4 to extract the remaining set of packages. But since we call yum4 from outside the chroot, we need to make some temporary changes. In the sample below, the target chroot is /srv/centos.
				

```bash
# rootdir="/srv/centos"
# mkdir -p "$rootdir" /etc/rpm
# echo "%_dbpath /var/lib/rpm" > /etc/rpm/macros.dbpath
# wget http://mirror.centos.org/centos/7/os/x86_64/Packages/centos-release-7-9.2009.0.el7.centos.x86_64.rpm
# rpm --nodeps --root "$rootdir" -i centos-release-7-9.2009.0.el7.centos.x86_64.rpm
rpm: RPM should not be used directly install RPM packages, use Alien instead!
rpm: However assuming you know what you are doing...
warning: centos-release-7-9.2009.0.el7.centos.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID f4a80eb5: NOKEY
# sed -i -e "s,gpgkey=file:///etc/,gpgkey=file://${rootdir}/etc/,g" $rootdir/etc/yum.repos.d/*.repo
# yum4 --assumeyes --installroot $rootdir groupinstall core
[...]
# sed -i -e "s,gpgkey=file://${rootdir}/etc/,gpgkey=file:///etc/,g" $rootdir/etc/yum.repos.d/*.repo
# chroot /srv/centos/
[root@testsystem /]#
```

[PrevChapterÂ 12.Â Advanced Administration](advanced-administration.en.md)[Up](#)[Home](index.en.md)[Next12.3.Â Automated Installation](sect.automated-installation.en.md)