---
title: "B.5. Sample script to change the default Bind installation."
source: "https://www.debian.org/doc/manuals/securing-debian-manual/bind-chuser.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "securing-debian-manual/bind-chuser.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - securing-debian-manual
aliases:
  - "B.5. Sample script to change the default Bind installation."
---
[](http://www.debian.org)[](http://www.debian.org/doc/)

[Prev](bridge-fw.en.md)Securing Debian Manual[Next](fw-security-update.en.md)

## B.5.Â Sample script to change the default Bind installation.

			This script automates the procedure for changing the bind version 8 name server's default installation so that it does not run as the superuser. Notice that bind version 9 in Debian already does this by default [[78]](#ftn.idm6384) , and you are much better using that version than bind version 8.
		
			This script is here for historical purposes and to show how you can automate this kind of changes system-wide. The script will create the user and groups defined for the name server and will modify both /etc/default/bind and /etc/init.d/bind so that the program will run with that user. Use with extreme care since it has not been tested thoroughly.
		
			You can also create the users manually and use the patch available for the default init.d script attached to [http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=157245](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=157245). 

```bash
#!/bin/sh
  # Change the default Debian bind v8 configuration to have it run
  # with a non-root user and group.
  # 
  # DO NOT USER this with version 9, use debconf for configure this instead
  #
  # WARN: This script has not been tested thoroughly, please
  # verify the changes made to the INITD script

  # (c) 2002 Javier Fernandez-Sanguino Pena
  #
  #    This program is free software; you can redistribute it and/or modify
  #    it under the terms of the GNU General Public License as published by
  #    the Free Software Foundation; either version 1, or (at your option)
  #    any later version.
  #
  #    This program is distributed in the hope that it will be useful,
  #    but WITHOUT ANY WARRANTY; without even the implied warranty of
  #    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
  #    GNU General Public License for more details.
  #
  #     Please see the file `COPYING' for the complete copyright notice.
  #

  restore() {
  # Just in case, restore the system if the changes fail
    echo "WARN: Restoring to the previous setup since I'm unable to properly change it."
    echo "WARN: Please check the $INITDERR script."
    mv $INITD $INITDERR
    cp $INITDBAK $INITD
  }

  USER=named
  GROUP=named
  INITD=/etc/init.d/bind
  DEFAULT=/etc/default/bind
  INITDBAK=$INITD.preuserchange
  INITDERR=$INITD.changeerror
  AWKS="awk ' /\/usr\/sbin\/ndc reload/ { print \"stop; sleep 2; start;\"; noprint = 1; } /\\\\$/ { if ( noprint != 0 ) { noprint = noprint + 1;} } /^.*$/ { if ( noprint != 0 ) { noprint = noprint - 1; } else { print \$0; } } '"

  [ `id -u` -ne 0 ] && {
    echo "This program must be run by the root user"
    exit 1
  }

  RUNUSER=`ps eo user,fname |grep named |cut -f 1 -d " "`

  if [ "$RUNUSER" = "$USER" ] 
  then
    echo "WARN: The name server running daemon is already running as $USER"
    echo "ERR:  This script will not do any changes to your setup."
    exit 1
  fi
  if [ ! -f "$INITD" ]
  then
    echo "ERR:  This system does not have $INITD (which this script tries to change)"
    RUNNING=`ps eo fname |grep named`
    [ -z "$RUNNING" ] && \
      echo "ERR:  In fact the name server daemon is not even running (is it installed?)"
    echo "ERR:  No changes will be made to your system"
    exit 1
  fi

  # Check if there are options already setup 
  if [ -e "$DEFAULT" ]
  then
    if grep -q ^OPTIONS $DEFAULT; then
      echo "ERR: The $DEFAULT file already has options set."
      echo "ERR:  No changes will be made to your system"
    fi
  fi

  # Check if named group exists
  if [ -z "`grep $GROUP /etc/group`" ] 
  then
    echo "Creating group $GROUP:"
    addgroup $GROUP
  else
    echo "WARN: Group $GROUP already exists. Will not create it"
  fi
  # Same for the user
  if [ -z "`grep $USER /etc/passwd`" ] 
  then
    echo "Creating user $USER:"
    adduser --system --home /home/$USER \
      --no-create-home --ingroup $GROUP \
      --disabled-password --disabled-login $USER
  else
    echo "WARN: The user $USER already exists. Will not create it"
  fi

  # Change the init.d script

  # First make a backup (check that there is not already
  # one there first)
  if [ ! -f $INITDBAK ] 
  then
    cp $INITD $INITDBAK
  fi

  # Then use it to change it
  cat $INITDBAK |
  eval $AWKS > $INITD

  # Now put the options in the /etc/default/bind file:
  cat >>$DEFAULT <<EOF
# Make bind run with the user we defined
OPTIONS="-u $USER -g $GROUP"
EOF

  echo "WARN: The script $INITD has been changed, trying to test the changes."
  echo "Restarting the named daemon (check for errors here)."

  $INITD restart
  if [ $? -ne 0 ] 
  then
    echo "ERR:  Failed to restart the daemon."
    restore
    exit 1
  fi

  RUNNING=`ps eo fname |grep named`
  if [ -z "$RUNNING" ] 
  then
    echo "ERR:  Named is not running, probably due to a problem with the changes."
    restore
    exit 1
  fi

  # Check if it's running as expected
  RUNUSER=`ps eo user,fname |grep named |cut -f 1 -d " "`

  if [ "$RUNUSER" = "$USER" ] 
  then
    echo "All has gone well, named seems to be running now as $USER."
  else
    echo "ERR:  The script failed to automatically change the system."
    echo "ERR:  Named is currently running as $RUNUSER."
    restore
    exit 1
  fi

  exit 0
```

			The previous script, run on Woody's (Debian 3.0) custom bind (version 8), will modify the initd file after creating the 'named' user and group and will
		[[78]](#idm6384)
				Since version 9.2.1-5. That is, since Debian release sarge.
			[PrevB.4.Â Setting up a bridge firewall](bridge-fw.en.md)[Up](#)[Home](index.en.md)[NextB.6.Â Security update protected by a firewall](fw-security-update.en.md)