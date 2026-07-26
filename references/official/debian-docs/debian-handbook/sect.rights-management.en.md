---
title: "9.3.2. ACLs - Access Control Lists"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.rights-management.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.rights-management.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.3.2. ACLs - Access Control Lists"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.remote-login.en.md)The Debian Administrator's Handbook[Next](sect.administration-interfaces.en.md)

## 9.3.Â Managing Rights

			Linux is definitely a multi-user system, so it is necessary to provide a permission system to control the set of authorized operations on files and directories, which includes all the system resources and devices (on a Unix system, any device is represented by a file or directory). This principle is common to all Unix systems, but a reminder is always useful, especially as there are some interesting and relatively unknown advanced uses.
		

### 9.3.1.Â Owners and Permissions

				Each file or directory has specific permissions for three categories of users:
			
						its owner (symbolized by u as in âuserâ);
					
						its owner group (symbolized by g as in âgroupâ), representing all the members of the group;
					
						the others (symbolized by o as in âotherâ).
					
				Three basic types of rights can be combined:
			
						reading (symbolized by r as in âreadâ);
					
						writing (or modifying, symbolized by w as in âwriteâ);
					
						executing (symbolized by x as in âeXecuteâ).
					
				In the case of a file, these rights are easily understood: read access allows reading the content (including copying), write access allows changing it, and execute access allows you to run it (which will only work if it is a program).
			

SECURITY setuid and setgid executables

				Two particular rights are relevant to executable files: setuid and setgid (symbolized with the letter âsâ). Note that we frequently speak of âbitâ, since each of these Boolean values can be represented by a 0 or a 1. These two rights allow any user to execute the program with the rights of its owner or its group, respectively. This mechanism grants access to features requiring higher level permissions than those you would usually have.
			
				Since a setuid root program is systematically run under the super-user identity, it is very important to ensure it is secure and reliable. Indeed, a user who would manage to subvert it to call a command of their choice could then impersonate the root user and have all rights on the system.
			
				If you require running a program under a different user or if a program requires higher permissions, the sudo, su, or runuser commands are usually better choices than using these bits (see [SectionÂ 8.9.4, âSharing Administrator Rightsâ](sect.config-misc.en.md#sect.sharing-admin-rights)).
			
				A directory is handled differently. Read access gives the right to consult the list of its entries (files and directories), write access allows creating or deleting files, and execute access allows crossing through it (especially to go there with the cd command). Being able to cross through a directory without being able to read it gives permission to access the entries therein that are known by name, but not to find them if you do not know their existence or their exact name.
			

SECURITY setgid directory and sticky bit

				The setgid bit also applies to directories. Any newly-created item in such directories is automatically assigned the owner group of the parent directory, instead of inheriting the creator's main group as usual. This setup avoids the user having to change its main group (with the newgrp command) when working in a file tree shared between several users of the same dedicated group.
			
				The âstickyâ bit (symbolized by the letter âtâ) is a permission that is only useful in directories. It is especially used for temporary directories where everybody has write access (such as /tmp/): it restricts deletion of files so that only their owner (or the owner of the parent directory) can do it. Lacking this, everyone could delete other users' files in /tmp/.
			
				Three commands control the permissions associated with a file:
			
chown user file changes the owner of the file;
					
chgrp group file alters the owner group;
					
chmod rights file changes the permissions for the file.
					
				There are two ways of presenting rights. Among them, the symbolic representation is probably the easiest to understand and remember. It involves the letter symbols mentioned above. You can define rights for each category of users (u/g/o), by setting them explicitly (with =), by adding (+), or subtracting (-). Thus the u=rwx,g+rw,o-r formula gives the owner read, write, and execute rights, adds read and write rights for the owner group, and removes read rights for other users. Rights not altered by the addition or subtraction in such a command remain unmodified. The letter a, for âallâ, covers all three categories of users, so that a=rx grants all three categories the same rights (read and execute, but not write).
			
				The (octal) numeric representation associates each right with a value: 4 for read, 2 for write, and 1 for execute. We associate each combination of rights with the sum of the figures. Each value is then assigned to different categories of users by putting them end to end in the usual order (owner, group, others).
			
				For instance, the chmod 754 file command will set the following rights: read, write and execute for the owner (since 7 = 4 + 2 + 1); read and execute for the group (since 5 = 4 + 1); read-only for others. The 0 (zero) means no rights; thus chmod 600 file allows for read/write rights for the owner, and no rights for anyone else. The most frequent right combinations are 755 for executable files and directories, and 644 for data files.
			
				To represent special rights, you can prefix a fourth digit to this number according to the same principle, where the setuid, setgid and sticky bits are 4, 2 and 1, respectively. chmod 4754 will associate the setuid bit with the previously described rights.
			
				Note that the use of octal notation only allows to set all the rights at once on a file; you cannot use it to simply add a new right, such as read access for the group owner, since you must take into account the existing rights and compute the new corresponding numerical value.
			

TIP Recursive operation

				Sometimes we have to change rights for an entire file tree. All the commands above have a -R option to operate recursively in sub-directories.
			
				The distinction between directories and files sometimes causes problems with recursive operations. That is why the âXâ letter has been introduced in the symbolic representation of rights. It also represents the right to execute, but it applies differently to directories and files.
			
				For directories it adds executable permissions to the chosen user(s). For files, it adds the executable bit only if at least one of the users (owner, group, or others) already has executable permissions. Let's demonstrate it, because this bit can be confusing:
			

```bash
$ ls test.txt
  -rw-r--r-- 1 daniel daniel 0 18. Nov 01:07 test.txt
  $ chmod u+X test.txt
  $ ls test.txt
  -rw-r--r-- 1 daniel daniel 0 18. Nov 01:07 test.txt
  $ chmod o+x test.txt
  $ ls test.txt
  -rw-r--r-x 1 daniel daniel 0 18. Nov 01:07 test.txt
  $ chmod u+X test.txt
  $ ls test.txt
  -rwxr--r-x 1 daniel daniel 0 18. Nov 01:07 test.txt
```

				The example shows a typical file with its default permissions: its owner can read and write it, the owner's group and all other users can read it. The next operation (u+X) won't add executable permissions for the owner of the file, because permissions to execute have not been assigned. The operation has no effect on the file. Next we assign execute rights for "other" users and repeat the operation. This time it is successful, because at least one user group already had executable permissions.
			
				It is a misconception that this bit will only affect directories. If files and directories have mixed permissions, it is often a good idea to use the find command to locate the targets you want to operate on.
			

TIP Changing the user and group together

				Frequently you want to change the group of a file at the same time that you change the owner. The chown command has a special syntax for that: chown user:group file. This syntax can also be used to recursively (-R) change the ownership of a whole directory.
			

GOING FURTHER umask

				When an application creates a file, it assigns indicative permissions, knowing that the system automatically removes certain rights, given by the command umask. Enter umask in a shell; you will see a mask such as 0022. This is simply an octal representation of the rights to be systematically removed. In this case, the write right for the group and other users: With the above umask value, the default for directories 777 becomes 755 and the default permissions for files 666 become 644.
			
				The system's default value is handled by pam_umask(8) and /etc/login.defs.
			
				If you give it a new octal value, the umask command modifies the mask. Used in a shell initialization file (for example, ~/.bashrc or ~/.profile), it will effectively change the default mask for your work sessions.
			

### 9.3.2.Â ACLs - Access Control Lists

				Many filesystems, e.g. Btrfs, Ext3, Ext4, JFS, XFS, etc., support the use of Access Control Lists (ACLs). These extend the basic features of file ownership and permission, described in the previous section, and allow for a more fine-grained control of each (file) object. For example: A user wants to share a file with another user and that user should only be able to read the file, but not write or change it.
			
				For some of the filesystems, the usage of ACLs is enabled by default (e.g. Btrfs, Ext3, Ext4). For other filesystems or older systems it must be enabled using the acl mount option - either in the mount command directly or in /etc/fstab. In the same way the usage of ACLs can be disabled by using the noacl mount option. For Ext* filesystems one can also use the tune2fs -o [no]acl /dev/device command to enable/disable the usage of ACLs by default. The default values for each filesystem can usually be found in their homonym manual pages in section 5 (filesystem(5)) or in mount(8).
			
				After enabling ACLs, permissions can be set using the setfacl(1) command, while getfacl(1) allows one to retrieve the ACLs for a given object or path. These commands are part of the acl package. With setfacl one can also configure newly created files or directories to inherit permissions from the parent directory. It is important to note that ACLs are processed in their order and that an earlier entry that fits the situation has precedence over later entries.
			
				If a file has ACLs set, the output of the ls -l command will show a plus-sign after the traditional permissions. When using ACLs, the chmod command behaves slightly different, and umask might be ignored. The extensive documentation, e.g. acl(5) contains more information.
			[Prev9.2.Â Remote Login](sect.remote-login.en.md)[Up](#)[Home](index.en.md)[Next9.4.Â Administration Interfaces](sect.administration-interfaces.en.md)