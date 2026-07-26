---
title: "9.2. Creating users and groups for software daemons"
source: "https://www.debian.org/doc/manuals/securing-debian-manual/bpp-lower-privs.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/bpp-lower-privs.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "9.2. Creating users and groups for software daemons"
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](ch09.en.md)Securing Debian Manual[Next](ch10.en.md)

## 9.2.Â Creating users and groups for software daemons

			If your software runs a daemon that does not need root privileges, you need to create a user for it. There are two kind of Debian users that can be used by packages: static uids (assigned by base-passwd, for a list of static users in Debian see [SectionÂ 12.1.1.12, âOperating system users and groupsâ](ch12.en.md#faq-os-users)) and dynamic uids in the range assigned to system users.
		
			In the first case, you need to ask for a user or group id to the base-passwd. Once the user is available there the package needs to be distributed including a proper versioned depends to the base-passwd package.
		
			In the second case, you need to create the system user either in the preinst or in the postinst and make the package depend on adduser (>= 3.11).
		
			The following example code creates the user and group the daemon will run as when the package is installed or upgraded: 

```bash
[...]
case "$1" in
  install|upgrade)

  # If the package has default file it could be sourced, so that
  # the local admin can overwrite the defaults

  [ -f "/etc/default/packagename" ] && . /etc/default/packagename

  # Sane defaults:

  [ -z "$SERVER_HOME" ] && SERVER_HOME=server_dir
  [ -z "$SERVER_USER" ] && SERVER_USER=server_user
  [ -z "$SERVER_NAME" ] && SERVER_NAME="Server description"
  [ -z "$SERVER_GROUP" ] && SERVER_GROUP=server_group

  # Groups that the user will be added to, if undefined, then none.
  ADDGROUP=""

  # create user to avoid running server as root
  # 1. create group if not existing
  if ! getent group | grep -q "^$SERVER_GROUP:" ; then
     echo -n "Adding group $SERVER_GROUP.."
     addgroup --quiet --system $SERVER_GROUP 2>/dev/null ||true
     echo "..done"
  fi
  # 2. create homedir if not existing
  test -d $SERVER_HOME || mkdir $SERVER_HOME
  # 3. create user if not existing
  if ! getent passwd | grep -q "^$SERVER_USER:"; then
    echo -n "Adding system user $SERVER_USER.."
    adduser --quiet \
            --system \
            --ingroup $SERVER_GROUP \
            --no-create-home \
            --disabled-password \
            $SERVER_USER 2>/dev/null || true
    echo "..done"
  fi
  # 4. adjust passwd entry
  usermod -c "$SERVER_NAME" \
          -d $SERVER_HOME   \
          -g $SERVER_GROUP  \
             $SERVER_USER
  # 5. adjust file and directory permissions
  if ! dpkg-statoverride --list $SERVER_HOME >/dev/null
  then
      chown -R $SERVER_USER:adm $SERVER_HOME
      chmod u=rwx,g=rxs,o= $SERVER_HOME
  fi
  # 6. Add the user to the ADDGROUP group
  if test -n $ADDGROUP
  then
      if ! groups $SERVER_USER | cut -d: -f2 | \
         grep -qw $ADDGROUP; then
           adduser $SERVER_USER $ADDGROUP
      fi
  fi
  ;;
  configure)

[...]
```

			You have to make sure that the init.d script file: 
			
						Starts the daemon dropping privileges: if the software does not do the setuid(2) or seteuid(2) call itself, you can use the --chuid call of start-stop-daemon.
					
						Stops the daemon only if the user id matches, you can use the start-stop-daemon --user option for this.
					
						Does not run if either the user or the group do not exist: 

```bash
if ! getent passwd | grep -q "^server_user:"; then
     echo "Server user does not exist. Aborting" >&2
     exit 1
  fi
  if ! getent group | grep -q "^server_group:" ; then
     echo "Server group does not exist. Aborting" >&2
     exit 1
  fi
```

			If the package creates the system user it can remove it when it is purged in its postrm. This has some drawbacks, however. For example, files created by it will be orphaned and might be taken over by a new system user in the future if it is assigned the same uid[[59]](#ftn.idm3999). Consequently, removing system users on purge is not yet mandatory and depends on the package needs. If unsure, this action could be handled by asking the administrator for the prefered action when the package is installed (i.e. through debconf).
		
			Maintainers that want to remove users in their postrm scripts are referred to the deluser/deluser --system option.
		
			Running programs with a user with limited privileges makes sure that any security issue will not be able to damage the full system. It also follows the principle of least privilege. Also consider you can limit privileges in programs through other mechanisms besides running as non-root[[60]](#ftn.idm4011). For more information, read the [http://www.dwheeler.com/secure-programs/Secure-Programs-HOWTO/minimize-privileges.html](http://www.dwheeler.com/secure-programs/Secure-Programs-HOWTO/minimize-privileges.html) chapter of the Secure Programming for Linux and Unix HOWTO book.
		[[59]](#idm3999)
				Some relevant threads discussing these drawbacks include [http://lists.debian.org/debian-mentors/2004/10/msg00338.html](http://lists.debian.org/debian-mentors/2004/10/msg00338.html) and [http://lists.debian.org/debian-devel/2004/05/msg01156.html](http://lists.debian.org/debian-devel/2004/05/msg01156.html)
[[60]](#idm4011)
				You can even provide a SELinux policy for it
			[PrevChapterÂ 9.Â Developer's Best Practices for OS Secu...](ch09.en.md)[Up](#)[Home](index.en.md)[NextChapterÂ 10.Â Before the compromise](ch10.en.md)