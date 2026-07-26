---
title: "11.4.3. NFS Client"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.nfs-file-server.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.nfs-file-server.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "11.4.3. NFS Client"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.ftp-file-server.en.md)The Debian Administrator's Handbook[Next](sect.windows-file-server-with-samba.en.md)

## 11.4.Â NFS File Server

			NFS (Network File System) is a protocol allowing remote access to a filesystem through the network. All Unix systems can work with this protocol.
		

SPECIFIC CASE Microsoft Windows and NFS Shares

			When older or (so called) "Home" variants of Windows are involved, usually Samba ([SectionÂ 11.5, âSetting Up Windows Shares with Sambaâ](sect.windows-file-server-with-samba.en.md)) must be used instead of NFS. Modern Windows Server and "Pro" or "Enterprise" Desktop solutions however have built-in support for NFS. After installation of the "Services for NFS" components NFS shares can be accessed and temporarily or permanently mounted like any other network share. Be aware of possible encoding issues in file names.
		
			As an alternative Debian can be installed on Windows 10 Pro and higher. It requires the installation of the Windows Subsystem for Linux (WSL) component and the Debian app from the Windows store. âÂ [https://www.microsoft.com/en-us/p/debian/9msvkqc78pk6?](https://www.microsoft.com/en-us/p/debian/9msvkqc78pk6?)

			NFS is a very useful tool but, historically, it has suffered from many limitations, most of which have been addressed with version 4 of the protocol. The downside is that the latest version of NFS is harder to configure when you want to make use of basic security features such as authentication and encryption since it relies on Kerberos for those parts. And without those, the NFS protocol must be restricted to a trusted local network since data goes over the network unencrypted (a sniffer can intercept it) and access rights are granted based on the client's IP address (which can be spoofed).
		

DOCUMENTATION NFS HOWTO

			Good documentation to deploy NFSv4 is rather scarce. Here are some pointers with content of varying quality but that should at least give some hints on what should be done. âÂ [https://help.ubuntu.com/community/NFSv4Howto](https://help.ubuntu.com/community/NFSv4Howto) âÂ [https://wiki.linux-nfs.org/wiki/index.php/Nfsv4_configuration](https://wiki.linux-nfs.org/wiki/index.php/Nfsv4_configuration)

### 11.4.1.Â Securing NFS

				If you don't use the Kerberos-based security features, it is vital to ensure that only the machines allowed to use NFS can connect to the various required RPC servers, because the basic protocol trusts the data received from the network. The firewall must also block IP spoofing so as to prevent an outside machine from acting as an inside one, and access to the appropriate ports must be restricted to the machines meant to access the NFS shares.
			

BACK TO BASICS RPC

				RPC (Remote Procedure Call) is a Unix standard for remote services. NFS is one such service.
			
				RPC services register to a directory known as the portmapper. A client wishing to perform an NFS query first addresses the portmapper (on port 111, either TCP or UDP), and asks for the NFS server; the reply usually mentions port 2049 (the default for NFS). Not all RPC services necessarily use a fixed port.
			
				Older versions of the protocol required other RPC services which used dynamically assigned ports. Fortunately, with NFS version 4, only port 2049 (for NFS) and 111 (for the portmapper) are needed and they are thus easy to firewall.
			

### 11.4.2.Â NFS Server

				The NFS server is part of the Linux kernel; in kernels provided by Debian it is built as a kernel module. If the NFS server is to be run automatically on boot, the nfs-kernel-server package should be installed; it contains the relevant start-up scripts.
			
				The NFS server configuration file(s), /etc/exports and /etc/exports.d/, lists the directories that are made available over the network (exported). For each NFS share, only the given list of machines is granted access. More fine-grained access control can be obtained with a few options. The syntax for this file is quite simple:
			

```bash
/directory/to/share machine1(option1,option2,...) machine2(...) ...
```

				Note that with NFSv4, all exported directories must be part of a single hierarchy and that the root directory of that hierarchy must be exported and identified with the option fsid=0 or fsid=root.
			
				Each machine can be identified either by its DNS name or its IP address. Whole sets of machines can also be specified using either a syntax such as *.falcot.com or an IP address range such as 192.168.0.0/255.255.255.0 or 192.168.0.0/24.
			
				Directories are made available as read-only by default (or with the ro option). The rw option allows read-write access. NFS clients typically connect from a port restricted to root (in other words, below 1024); this restriction can be lifted by the insecure option (the secure option is implicit, but it can be made explicit if needed for clarity).
			
				By default, the server only answers an NFS query when the current disk operation is complete (sync option); this can be disabled with the async option. Asynchronous writes increase performance a bit, but they decrease reliability since there is a data loss risk in case of the server crashing between the acknowledgment of the write and the actual write on disk. Since the default value changed recently (as compared to the historical value of NFS), an explicit setting is recommended.
			
				In order to not give root access to the filesystem to any NFS client, all queries appearing to come from a root user are considered by the server as coming from the nobody user. This behavior corresponds to the root_squash option, and is enabled by default. The no_root_squash option, which disables this behavior, is risky and should only be used in controlled environments. If all users should be mapped to the user nobody, use all_squash. The anonuid=uid and anongid=gid options allow specifying another fake user to be used instead of UID/GID 65534 (which corresponds to user nobody and group nogroup).
			
				With NFSv4, you can add a sec option to indicate the security level that you want: sec=sys is the default with no special security features, sec=krb5 enables authentication only, sec=krb5i adds integrity protection, and sec=krb5p is the most complete level which includes privacy protection (with data encryption). For this to work you need a working Kerberos setup (that service is not covered by this book).
			
				Other options are available; they are documented in the exports(5) manual page.
			

CAUTION First installation

				The file /etc/exports does not list any valid NFS shares on initial installation. Once either this file or any file in /etc/exports.d/ has been edited or added to contain valid entries, the NFS server must be restarted with either of the following commands:
			

```bash
# exportfs -ra
```

```bash
# systemctl start nfs-kernel-server
```

### 11.4.3.Â NFS Client

				As with other filesystems, integrating an NFS share into the system hierarchy requires mounting (and the nfs-common package). Since this filesystem has its peculiarities, a few adjustments were required in the syntax of the mount command and the /etc/fstab file.
			

ExampleÂ 11.19.Â Manually mounting with the mount command

```bash
# mount -t nfs4 -o rw,nosuid arrakis.internal.falcot.com:/shared /srv/shared
```

ExampleÂ 11.20.Â NFS entry in the /etc/fstab file

```bash
arrakis.internal.falcot.com:/shared /srv/shared nfs4 rw,nosuid 0 0
```

				The entry described above mounts, at system startup, the NFS directory /shared/ from the arrakis server into the local /srv/shared/ directory. Read-write access is requested (hence the rw parameter). The nosuid option is a protection measure that wipes any setuid or setgid bit from programs stored on the share. If the NFS share is only meant to store documents, another recommended option is noexec, which prevents executing programs stored on the share. Note that on the server, the shared directory is below the NFSv4 root export (for example /export/shared), it is not a top-level directory.
			
				The nfs(5) manual page describes all the options in some detail.
			[Prev11.3.Â FTP File Server](sect.ftp-file-server.en.md)[Up](#)[Home](index.en.md)[Next11.5.Â Setting Up Windows Shares with Samba](sect.windows-file-server-with-samba.en.md)