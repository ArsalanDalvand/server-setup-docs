---
title: "9.2.2. Using Remote Graphical Desktops"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.remote-login.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.remote-login.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.2.2. Using Remote Graphical Desktops"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](unix-services.en.md)The Debian Administrator's Handbook[Next](sect.rights-management.en.md)

## 9.2.Â Remote Login

			It is essential for an administrator to be able to connect to a computer remotely. Servers, confined in their own room, are rarely equipped with permanent keyboards and monitors â but they are connected to the network.
		

BACK TO BASICS Client, server

			A system where several processes communicate with each other is often described with the âclient/serverâ metaphor. The server is the program that takes requests coming from a client and executes them. It is the client that controls operations, the server doesn't take any initiative of its own.
		

### 9.2.1.Â Secure Remote Login: SSH

				The SSH (Secure SHell) protocol was designed with security and reliability in mind. Connections using SSH are secure: the partner is authenticated and all data exchanges are encrypted.
			

CULTURE Telnet and RSH are obsolete

				Before SSH, Telnet and RSH were the main tools used to login remotely. They are now largely obsolete and should no longer be used even if Debian still provides them.
			

VOCABULARY Authentication, encryption

				When you need to give a client the ability to conduct or trigger actions on a server, security is important. You must ensure the identity of the client; this is authentication. This identity usually consists of a password or passphrase that must be kept secret, or any other client could it. This is the purpose of encryption, which is a form of encoding that allows two systems to communicate confidential information on a public channel while protecting it from being readable to others.
			
				Authentication and encryption are often mentioned together, both because they are frequently used together, and because they are usually implemented with similar mathematical concepts.
			
				SSH also offers two file transfer services. scp is a command line tool that can be used like cp, except that any path to another machine is prefixed with the machine's name, followed by a colon.
			

```bash
$ scp file machine:/tmp/
```

sftp is an interactive command, similar to ftp. In a single session, sftp can transfer several files, and it is possible to manipulate remote files with it (delete, rename, change permissions, etc.).
			
				Debian uses OpenSSH, a free version of SSH maintained by the OpenBSD project (a free operating system based on the BSD kernel, focused on security) and fork of the original SSH software developed by the SSH Communications Security Corp company, of Finland. This company initially developed SSH as free software, but eventually decided to continue its development under a proprietary license. The OpenBSD project then created OpenSSH to maintain a free version of SSH.
			

BACK TO BASICS Fork

				A âforkâ, in the software field, means a new project that starts as a clone of an existing project, and that will compete with it. From there on, both software will usually quickly diverge in terms of new developments. A fork is often the result of disagreements within the development team.
			
				The option to fork a project is a direct result of the very nature of free software; a fork is a healthy event when it enables the continuation of a project as free software (for example in case of license changes). A fork arising from technical or personal disagreements is often a waste of human resources; another resolution would be preferable. Mergers of two projects that previously went through a prior fork are not unheard of.
			
				OpenSSH is split into two packages: the client part is in the openssh-client package, and the server is in the openssh-server package. The ssh meta-package depends on both parts and facilitates installation of both (apt install ssh), while the task-ssh-server, often chosen during the initial installation, depends on the server package only.
			

#### 9.2.1.1.Â Key-Based Authentication

					Each time someone logs in over SSH, the remote server asks for a password to authenticate the user. This can be problematic if you want to automate a connection, or if you use a tool that requires frequent connections over SSH. This is why SSH offers a key-based authentication system.
				
					The user generates a key pair on the client machine with ssh-keygen -t rsa; the so generated public key is stored in ~/.ssh/id_rsa.pub, while the corresponding private key is stored in ~/.ssh/id_rsa. The user can then use ssh-copy-id server to add their public key to the ~/.ssh/authorized_keys file on the server, or, if SSH access hasn't been enabled yet, they have to ask the administrator to add their key manually.
				
					If the private key was not protected with a âpassphraseâ at the time of its creation, all subsequent logins on the server will work without a password. Otherwise, the private key must be decrypted each time by entering the passphrase. Fortunately, ssh-agent allows us to keep private keys in memory to not have to regularly re-enter the password. For this, you simply use ssh-add (once per work session) provided that the session is already associated with a functional instance of ssh-agent. Debian activates it by default in graphical sessions, but this can be deactivated by changing /etc/X11/Xsession.options and commenting out use-ssh-agent. For a console session, you can manually start the agent with eval $(ssh-agent).
				

SECURITY Protection of the private key

					Whoever has the private key can login on the account thus configured. This is why access to the private key is protected by a âpassphraseâ. Someone who acquires a copy of a private key file (for example, ~/.ssh/id_rsa) still has to know this phrase in order to be able to use it. This additional protection is not, however, impregnable, and if you think that this file has been compromised, it is best to disable that key on the computers in which it has been installed (by removing it from the authorized_keys files) and replacing it with a newly generated key.
				

CULTURE OpenSSL flaw in Debian Etch

					The OpenSSL library, as initially provided in Debian Etch, had a serious problem in its random number generator (RNG). Indeed, the Debian maintainer had made a change so that applications using it would no longer generate warnings when analyzed by memory testing tools like valgrind. Unfortunately, this change also meant that the RNG was employing only one source of entropy corresponding to the process number (PID) whose 32,000 possible values do not offer enough randomness. âÂ [https://www.debian.org/security/2008/dsa-1571](https://www.debian.org/security/2008/dsa-1571)

					Specifically, whenever OpenSSL was used to generate a key, it always produced a key within a known set of hundreds of thousands of keys (32,000 multiplied by a small number of key lengths). This affected SSH keys, SSL keys, and X.509 certificates used by numerous applications, such as OpenVPN. A cracker had only to try all of the keys to gain unauthorized access. To reduce the impact of the problem, the SSH daemon was modified to refuse problematic keys that are listed in the openssh-blacklist and openssh-blacklist-extra packages. Additionally, the ssh-vulnkey command allows identification of possibly compromised keys in the system.
				
					A more thorough analysis of this incident brings to light that it is the result of multiple (small) problems, both within the OpenSSL project and with the Debian package maintainer. A widely used library like OpenSSL should â without modifications â not generate warnings when tested by valgrind. Furthermore, the code (especially the parts as sensitive as the RNG) should be better commented to prevent such errors. On Debian's side, the maintainer wanted to validate the modifications with the OpenSSL developers, but simply explained the modifications without providing the corresponding patch to review and failed to mention his role within Debian. Finally, the maintenance choices were sub-optimal: the changes made to the original code were not clearly documented; all the modifications were effectively stored in a Subversion repository, but they ended up all lumped into one single patch during creation of the source package.
				
					It is difficult under such conditions to find the corrective measures to prevent such incidents from recurring. The lesson to be learned here is that every divergence Debian introduces to upstream software must be justified, documented, submitted to the upstream project when possible, and widely publicized. It is from this perspective that the new source package format (â3.0 (quilt)â) and the Debian sources webservice were developed. âÂ [https://sources.debian.org](https://sources.debian.org)

#### 9.2.1.2.Â Cert-Based Authentication

					SSH keys cannot just be protected by a password (or not). An often unknown feature is that they can also be signed via certificate, both the host as well as the client keys. This approach comes with several advantages. Instead of maintaining an authorized_keys file per user as described in the previous section, the SSH server can be configured to trust all client keys signed by the same certificate (see also [SectionÂ 10.2.2, âPublic Key Infrastructure: easy-rsaâ](sect.x509-cert.en.md#sect.easy-rsa)) by using the TrustedUserCAKeys and HostCertificate directives in /etc/ssh/sshd_config.
				

```bash
TrustedUserCAKeys /etc/ssh/ssh_users_ca.pub

HostKey /etc/ssh/ssh_host_ecdsa_key
HostCertificate /etc/ssh/ssh_host_ecdsa_key-cert.pub
```

					Vice-versa the clients can also be configured to trust the host key signed by the same authority, making it easier to maintain the known_hosts file (even system wide via /etc/ssh/known_hosts).
				

```bash
@cert-authority *.falcot.com ssh-rsa AAAA[..]
```

					Both, public key and certificate authentication, can be used alongside each other.
				

#### 9.2.1.3.Â Using Remote X11 Applications

					The SSH protocol allows forwarding of graphical data (âX11â session, from the name of the most widespread graphical system in Unix); the server then keeps a dedicated channel for those data. Specifically, a graphical program executed remotely can be displayed on the X.org server of the local screen, and the whole session (input and display) will be secure. Since this feature allows remote applications to interfere with the local system, it is disabled by default. You can enable it by specifying X11Forwarding yes in the server configuration file (/etc/ssh/sshd_config). Finally, the user must also request it by adding the -X option to the ssh command-line.
				

GOING FURTHER Magic cookies in .Xauthority

					When a user connects via SSH and starts a remote X11 session, a so-called magic cookie is created and stored in the .Xauthority file in the user's home directory that initiated the connection. This cookie is used by xauth to authenticate the user during the X session. If the user impersonates another user on the system, e.g. using su or sudo, then the cookie is not copied automatically to the target user and the X server will refuse to start graphical applications under the target user's context. You will have to copy the magic cookie into the target user's home directory (by exporting and re-importing the cookie via xauth) to allow another user to start graphical programs as well during the X session.
				

#### 9.2.1.4.Â Creating Encrypted Tunnels with Port Forwarding

					Its -R and -L options allow ssh to create âencrypted tunnelsâ between two machines, securely forwarding a local TCP port (see sidebar [BACK TO BASICS TCP/UDP](network-infrastructure.en.md#sidebar.tcp-udp)) to a remote machine or vice versa.
				

VOCABULARY Tunnel

					The Internet, and most LANs that are connected to it, operate in packet mode and not in connected mode, meaning that a packet issued from one computer to another is going to be stopped at several intermediary routers to find its way to its destination. You can still simulate a connected operation where the stream is encapsulated in normal IP packets. These packets follow their usual route, but the stream is reconstructed unchanged at the destination. We call this a âtunnelâ, analogous to a road tunnel in which vehicles drive directly from the entrance (input) to the exit (output) without encountering any intersections, as opposed to a path on the surface that would involve intersections and changing direction.
				
					You can use this opportunity to add encryption to the tunnel: the stream that flows through it is then unrecognizable from the outside, but it is returned in decrypted form at the exit of the tunnel.
				
ssh -L 8000:server:25 intermediary establishes an SSH session with the intermediary host and listens to local port 8000 (see [FigureÂ 9.3, âForwarding a local port with SSHâ](sect.remote-login.en.md#figure.ssh-L)). For any connection established on this port, ssh will initiate a connection from the intermediary computer to port 25 on the server, and will bind both connections together.
				
ssh -R 8000:server:25 intermediary also establishes an SSH session to the intermediary computer, but it is on this machine that ssh listens to port 8000 (see [FigureÂ 9.4, âForwarding a remote port with SSHâ](sect.remote-login.en.md#figure.ssh-R)). Any connection established on this port will cause ssh to open a connection from the local machine on to port 25 of the server, and to bind both connections together.
				
					In both cases, connections are made to port 25 on the server host, which pass through the SSH tunnel established between the local machine and the intermediary machine. In the first case, the entrance to the tunnel is local port 8000, and the data move towards the intermediary machine before being directed to the server on the âpublicâ network. In the second case, the input and output in the tunnel are reversed; the entrance is port 8000 on the intermediary machine, the output is on the local host, and the data are then directed to the server. In practice, the server is usually either the local machine or the intermediary. That way SSH secures the connection from one end to the other.
				

FigureÂ 9.3.Â Forwarding a local port with SSH

FigureÂ 9.4.Â Forwarding a remote port with SSH

### 9.2.2.Â Using Remote Graphical Desktops

				VNC (Virtual Network Computing) allows remote access to graphical desktops.
			
				This tool is mostly used for technical assistance; the administrator can see the errors that the user is facing, and show them the correct course of action without having to stand by them.
			
				First, the user must authorize sharing their session. The GNOME graphical desktop environment includes that option via Settings â Sharing (contrary to previous versions of Debian, where the user had to install and run vino). For this to work network-manager must be managing the network used (e.g. enable the managed mode for devices handled by ifupdown in /etc/NetworkManager/NetworkManager.conf). KDE Plasma still requires using krfb to allow sharing an existing session over VNC. For other graphical desktop environments, the x11vnc or tightvncserver commands (from the Debian packages of the same name) or tigervncserver (tigervnc-standalone-server) serve the same purpose and provide the vnc-server virtual package; you can make either of them available to the user with an explicit menu or desktop entry.
			
				When the graphical session is made available by VNC, the administrator must connect to it with a VNC client. GNOME has vinagre and remmina for that, while the KDE project provides krdc (in the menu at K â Internet â Remote Desktop Client). There are other VNC clients that use the command line, such as xtightvncviewer from the homonym package or xtigervncviewer from the tigervnc-viewer Debian package. Once connected, the administrator can see what is going on, work on the machine remotely, and show the user how to proceed.
			

SECURITY VNC over SSH

				If you want to connect by VNC, and you don't want your data sent in clear text on the network, it is possible to encapsulate the data in an SSH tunnel (see [SectionÂ 9.2.1.4, âCreating Encrypted Tunnels with Port Forwardingâ](sect.remote-login.en.md#sect.ssh-port-forwarding)). You simply have to know that VNC uses port 5900 by default for the first screen (called âlocalhost:0â), 5901 for the second (called âlocalhost:1â), etc.
			
				The ssh -L localhost:5901:localhost:5900 -N -T machine command creates a tunnel between local port 5901 in the localhost interface and port 5900 of the machine host. The first âlocalhostâ restricts SSH to listening to only that interface on the local machine. The second âlocalhostâ indicates the interface on the remote machine which will receive the network traffic entering in âlocalhost:5901â. Thus vncviewer localhost:1 will connect the VNC client to the remote screen, even though you indicate the name of the local machine.
			
				When the VNC session is closed, remember to close the tunnel by also quitting the corresponding SSH session.
			

BACK TO BASICS Display manager

gdm3, kdm, lightdm, and xdm are Display Managers (they all provide the x-display-manager meta package). They take control of the graphical interface shortly after boot in order to provide the user a login screen. Once the user has logged in, they execute the programs needed to start a graphical work session.
			[PrevChapterÂ 9.Â Unix Services](unix-services.en.md)[Up](#)[Home](index.en.md)[Next9.3.Â Managing Rights](sect.rights-management.en.md)