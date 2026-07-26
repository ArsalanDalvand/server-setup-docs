---
title: "8.4.5. Group List:/etc/group"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.user-group-databases.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.user-group-databases.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "8.4.5. Group List:/etc/group"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.hostname-name-service.en.md)The Debian Administrator's Handbook[Next](sect.creating-accounts.en.md)

## 8.4.Â User and Group Databases

			The list of users is usually stored in the /etc/passwd file, while the /etc/shadow file stores hashed passwords. Both are text files, in a relatively simple format, which can be read and modified with a text editor. Each user is listed there on a line with several fields separated with a colon (â:â).
		

NOTE Editing system files

			The system files mentioned in this chapter are all plain text files, and can be edited with a text editor. Considering their importance to core system functionality, it is always a good idea to take extra precautions when editing system files. First, always make a copy or backup of a system file before opening or altering it. Second, on servers or machines where more than one person could potentially access the same file at the same time, take extra steps to guard against file corruption.
		
			For this purpose, it is enough to use the vipw command to edit the /etc/passwd file, or vigr to edit /etc/group. These commands lock the file in question prior to running the text editor, (vi by default, unless the EDITOR environment variable has been altered). The -s option in these commands allows editing the corresponding shadow file.
		

BACK TO BASICS Crypt, a one-way function

crypt is a one-way function that transforms a string (A) into another string (B) in a way that A cannot be derived from B. The only way to identify A is to test all possible values, checking each one to determine if transformation by the function will produce B or not. It uses up to 8 characters as input (string A) and generates a string of 13, printable, ASCII characters (string B).
		

### 8.4.1.Â User List: /etc/passwd

				Here is the list of fields in the /etc/passwd file:
			
						login, for example rhertzog;
					
password: this is a password encrypted by a one-way function (crypt), relying on DES, MD5, SHA-256 or SHA-512. The special value âxâ indicates that the encrypted password is stored in /etc/shadow;
					
uid: unique number identifying each user;
					
gid: unique number for the user's main group (Debian creates a specific group for each user by default);
					
GECOS: data field usually containing the user's full name;
					
						login directory, assigned to the user for storage of their personal files (the environment variable $HOME generally points here);
					
						program to execute upon login. This is usually a command interpreter (shell), giving the user free rein. If you specify /bin/false (which does nothing and returns control immediately), the user cannot login.
					
				As mentioned before, one can edit this file directly. But there are more elegant ways to apply changes, which are described in [SectionÂ 8.4.3, âModifying an Existing Account or Passwordâ](sect.user-group-databases.en.md#sect.account-modification).
			

BACK TO BASICS Unix group

				A Unix group is an entity including several users so that they can easily share files using the integrated permission system (by benefiting from the same rights). You can also restrict use of certain programs to a specific group.
			

### 8.4.2.Â The Hidden and Encrypted Password File: /etc/shadow

				The /etc/shadow file contains the following fields:
			
						login;
					
						encrypted password;
					
						several fields managing password expiration.
					
				One can expire passwords using this file or set the time until the account is disabled after the password has expired.
			

SECURITY /etc/shadow file security

/etc/shadow, unlike its alter-ego, /etc/passwd, cannot be read by regular users. Any hashed password stored in /etc/passwd is readable by anybody; a cracker could try to âbreakâ (or reveal) a password by one of several âbrute forceâ methods which, simply put, guess at commonly used combinations of characters. This attack â called a "dictionary attack" â is no longer possible on systems using /etc/shadow.
			

DOCUMENTATION /etc/passwd, /etc/shadow and /etc/group file formats

				These formats are documented in the following man pages: passwd(5), shadow(5), and group(5).
			

### 8.4.3.Â Modifying an Existing Account or Password

				The following commands allow modification of the information stored in specific fields of the user databases: passwd permits a regular user to change their password, which in turn, updates the /etc/shadow file (chpasswd allows administrators to update passwords for a list of users in batch mode); chfn (CHange Full Name), reserved for the super-user (root), modifies the GECOS field. chsh (CHange SHell) allows the user to change their login shell; however, available choices will be limited to those listed in /etc/shells; the administrator, on the other hand, is not bound by this restriction and can set the shell to any program of their choosing.
			
				Finally, the chage (CHange AGE) command allows the administrator to change the password expiration settings (the -l user option will list the current settings). You can also force the expiration of a password using the passwd -e user command, which will require the user to change their password the next time they log in.
			
				Besides these tools the usermod command allows to modify all the details mentioned above.
			

### 8.4.4.Â Disabling an Account

				You may find yourself needing to âdisable an accountâ (lock out a user), as a disciplinary measure, for the purposes of an investigation, or simply in the event of a prolonged or definitive absence of a user. A disabled account means the user cannot login or gain access to the machine. The account remains intact on the machine and no files or data are deleted; it is simply inaccessible. This is accomplished by using the command passwd -l user (lock). Re-enabling the account is done in similar fashion, with the -u option (unlock). This, however, only prevents password-based logins by the user. The user might still be able to access the system using an SSH key (if configured). To prevent even this possibility you have to expire the account as well using either chage -E 1user or usermod -e 1 user (giving a value of -1 in either of these commands will reset the expiration date to never). To (temporarily) disable all user accounts just create the file /etc/nologin.
			
				You can disable a user account not only by locking it as described above, but also by changing its default login shell (chsh -s shell user). With the latter changed to /usr/sbin/nologin, a user gets a polite message informing that a login is not possible, while /bin/false just exits while returning false. There is no switch to restore the previous shell. You have to get and keep that information before you change the setting. These shells are often used for system users which do not require any login availability.
			

GOING FURTHER NSS and system databases

				Instead of using the usual files to manage lists of users and groups, you could use other types of databases, such as LDAP or db, by using an appropriate NSS (Name Service Switch) module. The modules used are listed in the /etc/nsswitch.conf file, under the passwd, shadow and group entries. See [SectionÂ 11.7.3.1, âConfiguring NSSâ](sect.ldap-directory.en.md#sect.config-nss) for a specific example of the use of an NSS module by LDAP.
			

### 8.4.5.Â Group List: /etc/group

				Groups are listed in the /etc/group file, a simple textual database in a format similar to that of the /etc/passwd file, with the following fields:
			
						group name;
					
						password (optional): This is only used to join a group when one is not a usual member (with the newgrp or sg commands, see sidebar [BACK TO BASICS Working with several groups](sect.user-group-databases.en.md#sidebar.working-with-several-groups));
					
gid: unique group identification number;
					
						list of members: list of names of users who are members of the group, separated by commas.
					

BACK TO BASICS Working with several groups

				Each user may be a member of many groups; one of them is their âmain groupâ. A user's main group is, by default, created during initial user configuration. By default, each file that a user creates belongs to them, as well as to their main group. This is not always desirable; for example, when the user needs to work in a directory shared by a group other than their main group. In this case, the user needs to change their main group using one of the following commands: newgrp, which starts a new shell, or sg, which simply executes a command using the supplied alternate group. These commands also allow the user to join a group to which they do not belong. If the group is password protected, they will need to supply the appropriate password before the command is executed.
			
				Alternatively, the user can set the setgid bit on the directory, which causes files created in that directory to automatically belong to the correct group. For more details, see sidebar [SECURITY setgid directory and sticky bit](sect.rights-management.en.md#sidebar.setgid-dir).
			
				The id command displays the current state of a user, with their personal identifier (uid variable), current main group (gid variable), and the list of groups to which they belong (groups variable).
			
				The addgroup and delgroup commands add or delete a group, respectively. The groupmod command modifies a group's information (its gid or identifier). The command gpasswd group changes the password for the group, while the gpasswd -r group command deletes it.
			

TIP getent

				The getent (get entries) command checks the system databases the standard way, using the appropriate library functions, which in turn call the NSS modules configured in the /etc/nsswitch.conf file. The command takes one or two arguments: the name of the database to check, and a possible search key. Thus, the command getent passwd rhertzog will give the information from the user database regarding the user rhertzog.
			[Prev8.3.Â Setting the Hostname and Configuring the Nam...](sect.hostname-name-service.en.md)[Up](#)[Home](index.en.md)[Next8.5.Â Creating Accounts](sect.creating-accounts.en.md)