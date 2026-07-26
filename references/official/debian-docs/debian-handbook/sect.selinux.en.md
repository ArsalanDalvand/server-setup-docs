---
title: "14.5.4. Adapting the Rules"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.selinux.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.selinux.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "14.5.4. Adapting the Rules"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.apparmor.en.md)The Debian Administrator's Handbook[Next](sect.other-security-considerations.en.md)

## 14.5.Â Introduction to SELinux

### 14.5.1.Â Principles

				SELinux (Security Enhanced Linux) is a Mandatory Access Control system built on Linux's LSM (Linux Security Modules) interface. In practice, the kernel queries SELinux before each system call to know whether the process is authorized to do the given operation.
			
				SELinux uses a set of rules â collectively known as a policy â to authorize or forbid operations. Those rules are difficult to create. Fortunately, two standard policies (targeted and strict) are provided to avoid the bulk of the configuration work.
			
				With SELinux, the management of rights is completely different from traditional Unix systems. The rights of a process depend on its security context. The context is defined by the identity of the user who started the process, the role and the domain that the user carried at that time. The rights really depend on the domain, but the transitions between domains are controlled by the roles. Finally, the possible transitions between roles depend on the identity.
			

FigureÂ 14.1.Â Security contexts and Unix users

				In practice, during login, the user gets assigned a default security context (depending on the roles that they should be able to endorse). This defines the current domain, and thus the domain that all new child processes will carry. If you want to change the current role and its associated domain, you must call newrole -r role_r -t domain_t (there is usually only a single domain allowed for a given role, the -t parameter can thus often be left out). This command authenticates you by asking you to type your password. This feature forbids programs to automatically switch roles. Such changes can only happen if they are explicitly allowed in the SELinux policy.
			
				Obviously the rights do not apply to all objects (files, directories, sockets, devices, etc.). They can vary from object to object. To achieve this, each object is associated to a type (this is known as labeling). Domains' rights are thus expressed with sets of (dis)allowed operations on those types (and, indirectly, on all objects which are labeled with the given type).
			

EXTRA Domains and types are equivalent

				Internally, a domain is just a type, but a type that only applies to processes. That is why domains are suffixed with _t just like objects' types.
			
				By default, a program inherits its domain from the user who started it, but the standard SELinux policies expect many important programs to run in dedicated domains. To achieve this, those executables are labeled with a dedicated type (for example ssh is labeled with ssh_exec_t, and when the program starts, it automatically switches to the ssh_t domain). This automatic domain transition mechanism makes it possible to grant only the rights required by each program. It is a fundamental principle of SELinux.
			

FigureÂ 14.2.Â Automatic transitions between domains

IN PRACTICE Finding the security context

				To find the current security context in a shell, you should call id -Z.
			

```bash
$ id -Z
unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```

				If you receive a message saying id: --context (-Z) works only on an SELinux-enabled kernel instead, then you need to enable SELinux first by running selinux-activate and rebooting.
			
				To find the security context of a given process, you should use the Z option of ps.
			

```bash
$ ps axZ | grep vstfpd
system_u:system_r:ftpd_t:s0   2094 ?    Ss  0:00 /usr/sbin/vsftpd
```

				The first field contains the identity, the role, the domain and the MCS level, separated by colons. The MCS level (Multi-Category Security) is a parameter that intervenes in the setup of a confidentiality protection policy, which regulates the access to files based on their sensitivity. This feature will not be explained in this book.
			
				Finally, to find the type assigned to a file, you can use ls -Z.
			

```bash
$ ls -Z test /usr/bin/ssh
unconfined_u:object_r:user_home_t:s0 test
system_u:object_r:ssh_exec_t:s0 /usr/bin/ssh
```

				It is worth noting that the identity and role assigned to a file bear no special importance (they are never used), but for the sake of uniformity, all objects get assigned a complete security context.
			

### 14.5.2.Â Setting Up SELinux

				SELinux support is built into the standard kernels provided by Debian. The core Unix tools support SELinux without any modifications. It is thus relatively easy to enable SELinux.
			
				The apt install selinux-basics selinux-policy-defaulti auditd command will automatically install the packages required to configure an SELinux system.
			
				The selinux-policy-default package contains a set of standard rules. By default, this policy only restricts access for a few widely exposed services. The user sessions are not restricted and it is thus unlikely that SELinux would block legitimate user operations. However, this does enhance the security of system services running on the machine. To setup a policy equivalent to the old âstrictâ rules, you just have to disable the unconfined module (modules management is detailed further in this section).
			
				Once the policy has been installed, you should label all the available files (which means assigning them a type). This operation must be manually started with fixfiles relabel.
			
				The SELinux system is now ready. To enable it, you should add the selinux=1 security=selinux parameter to the Linux kernel. The audit=1 parameter enables SELinux logging which records all the denied operations. Finally, the enforcing=1 parameter brings the rules into application: without it SELinux works in its default permissive mode where denied actions are logged but still executed. You should thus modify the GRUB bootloader configuration file to append the desired parameters. One easy way to do this is to modify the GRUB_CMDLINE_LINUX variable in /etc/default/grub and to run update-grub. SELinux will be active after a reboot.
			
				It is worth noting that the selinux-activate script automates those operations and forces a labeling on next boot (which avoids new non-labeled files created while SELinux was not yet active and while the labeling was going on).
			

### 14.5.3.Â Managing an SELinux System

				The SELinux policy is a modular set of rules, and its installation detects and enables automatically all the relevant modules based on the already installed services. The system is thus immediately operational. However, when a service is installed after the SELinux policy, you must be able to manually enable the corresponding module. That is the purpose of the semodule command. Furthermore, you must be able to define the roles that each user can endorse, and this can be done with the semanage command.
			
				Those two commands can thus be used to modify the current SELinux configuration, which is stored in /etc/selinux/default/. Unlike other configuration files that you can find in /etc/, all those files must not be changed by hand. You should use the programs designed for this purpose.
			

GOING FURTHER More documentation

				Since the NSA doesn't provide any official documentation, the community set up a wiki to compensate. It brings together a lot of information, but you must be aware that most SELinux contributors are Fedora users (where SELinux is enabled by default). The documentation thus tends to deal specifically with that distribution. âÂ [https://selinuxproject.org](https://selinuxproject.org)

				You should also have a look at the dedicated Debian wiki page. âÂ [https://wiki.debian.org/SELinux](https://wiki.debian.org/SELinux)

#### 14.5.3.1.Â Managing SELinux Modules

					Available SELinux modules are stored in the /usr/share/selinux/default/ directory. To enable one of these modules in the current configuration, you should use semodule -i module.pp.bz2. The pp.bz2 extension stands for policy package (compressed with bzip2).
				
					Removing a module from the current configuration is done with semodule -r module. Finally, the semodule -l command lists the modules which are currently installed. It also outputs their version numbers. Modules can be selectively enabled with semodule -e and disabled with semodule -d.
				

```bash
# semodule -i /usr/share/selinux/default/abrt.pp.bz2
libsemanage.semanage_direct_install_info: abrt module will be disabled after install as there is a disabled instance of this module present in the system.
# semodule -l
accountsd
acct
[...]
# semodule -e abrt
# semodule -d accountsd
# semodule -l
abrt
acct
[...]
# semodule -r abrt
libsemanage.semanage_direct_remove_key: abrt module at priority 100 is now active.
```

semodule immediately loads the new configuration unless you use its -n option. It is worth noting that the program acts by default on the current configuration (which is indicated by the SELINUXTYPE variable in /etc/selinux/config), but that you can modify another one by specifying it with the -s option.
				

#### 14.5.3.2.Â Managing Identities

					Every time that a user logs in, they get assigned an SELinux identity. This identity defines the roles that they will be able to endorse. Those two mappings (from the user to the identity and from this identity to roles) are configurable with the semanage command.
				
					You should definitely read the semanage(8) manual page. All the managed concepts have their own manual page; for instance, semanage-login(8). Even if the command's syntax tends to be similar for all the concepts which are managed, it is recommended to read its manual page. You will find common options to most subcommands: -a to add, -d to delete, -m to modify, -l to list, and -t to indicate a type (or domain).
				
semanage login -l lists the current mapping between user identifiers and SELinux identities. Users that have no explicit entry get the identity indicated in the __default__ entry. The semanage login -a -s user_u user command will associate the user_u identity to the given user. Finally, semanage login -d user drops the mapping entry assigned to this user.
				

```bash
# semanage login -a -s user_u rhertzog
# semanage login -l

Login Name           SELinux User         MLS/MCS Range        Service

__default__          unconfined_u         s0-s0:c0.c1023       *
rhertzog             user_u               s0                   *
root                 unconfined_u         s0-s0:c0.c1023       *
# semanage login -d rhertzog
```

semanage user -l lists the mapping between SELinux user identities and allowed roles. Adding a new identity requires to define both the corresponding roles and a labeling prefix which is used to assign a type to personal files (/home/user/*). The prefix must be picked among user, staff, and sysadm. The âstaffâ prefix results in files of type âstaff_home_dir_tâ. Creating a new SELinux user identity is done with semanage user -a -R roles -P prefix identity. Finally, you can remove an SELinux user identity with semanage user -d identity.
				

```bash
# semanage user -a -R 'staff_r user_r' -P staff test_u
# semanage user -l

                Labeling   MLS/       MLS/                          
SELinux User    Prefix     MCS Level  MCS Range                      SELinux Roles

root            sysadm     s0         s0-s0:c0.c1023                 staff_r sysadm_r system_r
staff_u         staff      s0         s0-s0:c0.c1023                 staff_r sysadm_r
sysadm_u        sysadm     s0         s0-s0:c0.c1023                 sysadm_r
system_u        user       s0         s0-s0:c0.c1023                 system_r
test_u          staff      s0         s0                             staff_r user_r
unconfined_u    unconfined s0         s0-s0:c0.c1023                 system_r unconfined_r
user_u          user       s0         s0                             user_r
# semanage user -d test_u
```

#### 14.5.3.3.Â Managing File Contexts, Ports and Booleans

					Each SELinux module provides a set of file labeling rules, but it is also possible to add custom labeling rules to cater to a specific case. For example, if you want the web server to be able to read files within the /srv/www/ file hierarchy, you could execute semanage fcontext -a -t httpd_sys_content_t "/srv/www(/.*)?" followed by restorecon -R /srv/www/. The former command registers the new labeling rules and the latter resets the file types according to the current labeling rules.
				
					Similarly, TCP/UDP ports are labeled in a way that ensures that only the corresponding daemons can listen to them. For instance, if you want the web server to be able to listen on port 8080, you should run semanage port -m -t http_port_t -p tcp 8080.
				
					Some SELinux modules export Boolean options that you can tweak to alter the behavior of the default rules. The getsebool utility can be used to inspect those options (getsebool boolean displays one option, and getsebool -a them all). The setsebool boolean value command changes the current value of a Boolean option. The -P option makes the change permanent, it means that the new value becomes the default and will be kept across reboots. The example below grants web servers an access to home directories (this is useful when users have personal websites in ~/public_html/).
				

```bash
# getsebool httpd_enable_homedirs
httpd_enable_homedirs --> off
# setsebool -P httpd_enable_homedirs on
# getsebool httpd_enable_homedirs
httpd_enable_homedirs --> on
```

### 14.5.4.Â Adapting the Rules

				Since the SELinux policy is modular, it might be interesting to develop new modules for (possibly custom) applications that lack them. These new modules will then complete the reference policy.
			
				To create new modules, the selinux-policy-dev package is required, as well as selinux-policy-doc. The latter contains the documentation of the standard rules (/usr/share/doc/selinux-policy-doc/html/) and sample files that can be used as templates to create new modules. Install those files and study them more closely:
			

```bash
$ cp /usr/share/doc/selinux-policy-doc/Makefile.example Makefile
$ cp /usr/share/doc/selinux-policy-doc/example.fc ./
$ cp /usr/share/doc/selinux-policy-doc/example.if ./
$ cp /usr/share/doc/selinux-policy-doc/example.te ./
```

				The .te file is the most important one. It defines the rules. The .fc file defines the âfile contextsâ, that is the types assigned to files related to this module. The data within the .fc file are used during the file labeling step. Finally, the .if file defines the interface of the module: it is a set of âpublic functionsâ that other modules can use to properly interact with the module that you're creating.
			

#### 14.5.4.1.Â Writing a .fc file

					Reading the below example should be sufficient to understand the structure of such a file. You can use regular expressions to assign the same security context to multiple files, or even an entire directory tree.
				

ExampleÂ 14.2.Â example.fc file

```bash
# myapp executable will have:
# label: system_u:object_r:myapp_exec_t
# MLS sensitivity: s0
# MCS categories: <none>

/usr/sbin/myapp         --      gen_context(system_u:object_r:myapp_exec_t,s0)
```

#### 14.5.4.2.Â Writing a .if File

					In the sample below, the first interface (âmyapp_domtransâ) controls who can execute the application. The second one (âmyapp_read_logâ) grants read rights on the application's log files.
				
					Each interface must generate a valid set of rules which can be embedded in a .te file. You should thus declare all the types that you use (with the gen_require macro), and use standard directives to grant rights. Note, however, that you can use interfaces provided by other modules. The next section will give more explanations about how to express those rights.
				

ExampleÂ 14.3.Â example.if File

```bash
## <summary>Myapp example policy</summary>
## <desc>
##      <p>
##              More descriptive text about myapp.  The desc
##              tag can also use p, ul, and ol
##              html tags for formatting.
##      </p>
##      <p>
##              This policy supports the following myapp features:
##              <ul>
##              <li>Feature A</li>
##              <li>Feature B</li>
##              <li>Feature C</li>
##              </ul>
##      </p>
## </desc>
#

########################################
## <summary>
##      Execute a domain transition to run myapp.
## </summary>
## <param name="domain">
##      <summary>
##      Domain allowed to transition.
##      </summary>
## </param>
#
interface(`myapp_domtrans',`
        gen_require(`
                type myapp_t, myapp_exec_t;
        ')

        domtrans_pattern($1,myapp_exec_t,myapp_t)
')

########################################
## <summary>
##      Read myapp log files.
## </summary>
## <param name="domain">
##      <summary>
##      Domain allowed to read the log files.
##      </summary>
## </param>
#
interface(`myapp_read_log',`
        gen_require(`
                type myapp_log_t;
        ')

        logging_search_logs($1)
        allow $1 myapp_log_t:file read_file_perms;
')
```

DOCUMENTATION Explanations about the reference policy

					The reference policy evolves like any free software project: based on volunteer contributions. The project is hosted by Tresys, one of the most active companies in the SELinux field. Their wiki contains explanations on how the rules are structured and how you can create new ones. âÂ [https://github.com/SELinuxProject/refpolicy/wiki/GettingStarted](https://github.com/SELinuxProject/refpolicy/wiki/GettingStarted)

#### 14.5.4.3.Â Writing a .te File

					Have a look at the example.te file:
				

GOING FURTHER The m4 macro language

					To properly structure the policy, the SELinux developers used a macro-command processor. Instead of duplicating many similar allow directives, they created âmacro functionsâ to use a higher-level logic, which also results in a much more readable policy.
				
					In practice, m4 is used to compile those rules. It does the opposite operation: it expands all those high-level directives into a huge database of allow directives.
				
					The SELinux âinterfacesâ are only macro functions which will be substituted by a set of rules at compilation time. Likewise, some rights are in fact sets of rights which are replaced by their values at compilation time.
				

```bash
policy_module(example,1.0.0)  # a non-base module name must match the file name

########################################
#
# Declarations
#

type myapp_t; 
type myapp_exec_t;
domain_type(myapp_t)
domain_entry_file(myapp_t, myapp_exec_t) 

type myapp_log_t;
logging_log_file(myapp_log_t) 

type myapp_tmp_t;
files_tmp_file(myapp_tmp_t)

########################################
#
# Myapp local policy
#

allow myapp_t myapp_log_t:file { read_file_perms append_file_perms }; 

allow myapp_t myapp_tmp_t:file manage_file_perms;
files_tmp_filetrans(myapp_t,myapp_tmp_t,file)
```

[](#example.te.module)

							The module must be identified by its name and version number. This directive is required.
						

[](#example.te.type)

							If the module introduces new types, it must declare them with directives like this one. Do not hesitate to create as many types as required rather than granting too many useless rights.
						

[](#example.te.domain)

							Those interfaces define the myapp_t type as a process domain that should be used by any executable labeled with myapp_exec_t. Implicitly, this adds an exec_type attribute on those objects, which in turn allows other modules to grant rights to execute those programs: for instance, the userdomain module allows processes with domains user_t, staff_t, and sysadm_t to execute them. The domains of other confined applications will not have the rights to execute them, unless the rules grant them similar rights (this is the case, for example, of dpkg with its dpkg_t domain).
						

[](#example.te.interface)

logging_log_file is an interface provided by the reference policy. It indicates that files labeled with the given type are log files which ought to benefit from the associated rules (for example, granting rights to logrotate so that it can manipulate them).
						

[](#example.te.allow)

							The allow directive is the base directive used to authorize an operation. The first parameter is the process domain which is allowed to execute the operation. The second one defines the object that a process of the former domain can manipulate. This parameter is of the form âtype:classâ where type is its SELinux type and class describes the nature of the object (file, directory, socket, fifo, etc.). Finally, the last parameter describes the permissions (the allowed operations).
						
							Permissions are defined as the set of allowed operations and follow this template: { operation1 operation2 }. However, you can also use macros representing the most useful permissions. The /usr/share/selinux/devel/include/support/obj_perm_sets.spt lists them.
						
							The following web page provides a relatively exhaustive list of object classes, and permissions that can be granted. âÂ [https://selinuxproject.org/page/ObjectClassesPerms](https://selinuxproject.org/page/ObjectClassesPerms)

					Now you just have to find the minimal set of rules required to ensure that the target application or service works properly. To achieve this, you should have a good knowledge of how the application works and of what kind of data it manages and/or generates.
				
					However, an empirical approach is possible. Once the relevant objects are correctly labeled, you can use the application in permissive mode: the operations that would be forbidden are logged but still succeed. By analyzing the logs, you can now identify the operations to allow. Here is an example of such a log entry:
				

```bash
avc:  denied  { read write } for  pid=1876 comm="syslogd" name="xconsole" dev=tmpfs ino=5510 scontext=system_u:system_r:syslogd_t:s0 tcontext=system_u:object_r:device_t:s0 tclass=fifo_file permissive=1
```

					To better understand this message, let us study it piece by piece.
				

TableÂ 14.1.Â Analysis of an SELinux trace

MessageDescription avc: denied An operation has been denied. { read write } This operation required the read and write permissions. pid=1876 The process with PID 1876 executed the operation (or tried to execute it). comm="syslogd" The process was an instance of the syslogd program. name="xconsole" The target object was named xconsole. Sometimes you can also have a âpathâ variable â with the full path â instead. dev=tmpfs The device hosting the target object is a tmpfs (an in-memory filesystem). For a real disk, you could see the partition hosting the object (for example, âsda3â). ino=5510 The object is identified by the inode number 5510. scontext=system_u:system_r:syslogd_t:s0 This is the security context of the process who executed the operation. tcontext=system_u:object_r:device_t:s0 This is the security context of the target object. tclass=fifo_file The target object is a FIFO file.
					By observing this log entry, it is possible to build a rule that would allow this operation. For example, allow syslogd_t device_t:fifo_file { read write }. This process can be automated, and it is exactly what the audit2allow command (of the policycoreutils package) offers. This approach is only useful if the various objects are already correctly labeled according to what must be confined. In any case, you will have to carefully review the generated rules and validate them according to your knowledge of the application. Effectively, this approach tends to grant more rights than are really required. The proper solution is often to create new types and to grant rights on those types only. It also happens that a denied operation isn't fatal to the application, in which case it might be better to just add a âdontauditâ rule to avoid the log entry despite the effective denial.
				

COMPLEMENTS No roles in policy rules

					It might seem weird that roles do not appear at all when creating new rules. SELinux uses only the domains to find out which operations are allowed. The role intervenes only indirectly by allowing the user to switch to another domain. SELinux is based on a theory known as Type Enforcement and the type is the only element that matters when granting rights.
				

#### 14.5.4.4.Â Compiling the Files

					Once the 3 files (example.if, example.fc, and example.te) match your expectations for the new rules, rename them to myapp.extension and run make NAME=devel to generate a module in the myapp.pp file (you can immediately load it with semodule -i myapp.pp). If several modules are defined, make will create all the corresponding .pp files.
				[Prev14.4.Â Introduction to AppArmor](sect.apparmor.en.md)[Up](#)[Home](index.en.md)[Next14.6.Â Other Security-Related Considerations](sect.other-security-considerations.en.md)