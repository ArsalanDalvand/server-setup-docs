---
title: "Synchronize cluster node clocks"
source: "https://redis.io/docs/latest/operate/rs/clusters/configure/sync-clocks/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/configure/sync-clocks"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Synchronize cluster node clocks"
---
# Synchronize cluster node clocks

Sync node clocks to avoid problems with internal custer communication.

            Redis Software
          

To avoid problems with internal cluster communications that can impact your data integrity,
make sure that the clocks on all of the cluster nodesÂ are synchronized using Chrony and/or NTP.

When you install Redis Software,
the install script checks if Chrony or NTP is running.
If they are not, the installation process asks for permission to configure a scheduled Cron job.
This should make sure that the node's clock is always synchronized.
If you did not confirm configuring this job during the installation process,
you must use the Network Time Protocol (NTP) regularly to make sure that all server clocks are synchronized.

To synchronize the server clock, run the command that is appropriate for your operating system.

## Set up NTP synchronization

To set up NTP synchronization, see the following sections for instructions for specific operating systems.

### Ubuntu 18.04 and Ubuntu 20.04

Install Chrony, a replacement for NTP:

```sh
sudo apt install chrony
```

Edit the Chrony configuration file:

```sh
sudo nano /etc/chrony/chrony.conf
```

Add `server pool.ntp.org` to the file, replace `pool.ntp.org` with your own NTP server, then save.

Restart the Chrony service:

```sh
sudo systemctl restart chrony
```

Check the Chrony service status:

```sh
sudo systemctl status chrony
```

For more details, refer to the official [Ubuntu 20.04 documentation](https://ubuntu.com/server/docs/network-ntp).

### RHEL 7

Install Chrony, a replacement for NTP:

```sh
sudo yum install chrony
```

Edit the Chrony configuration file:

```sh
sudo nano /etc/chrony.conf
```

Add `server pool.ntp.org` to the file, replace `pool.ntp.org` with your own NTP server, then save.

Enable and start the Chrony service:

```sh
sudo systemctl enable chronyd && sudo systemctl start chronyd
```

Check the Chrony service status:

```sh
sudo systemctl status chronyd
```

For more details, refer to the official [RHEL 7 documentation](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/system_administrators_guide/ch-configuring_ntp_using_the_chrony_suite#sect-Using_chrony).

### RHEL 8 and RHEL 9

Install Chrony, a replacement for NTP:

```sh
sudo dnf install chrony
```

Edit the Chrony configuration file:

```sh
sudo nano /etc/chrony.conf
```

Add `server pool.ntp.org` to the file, replace `pool.ntp.org` with your own NTP server, then save.

Enable and start the Chrony service:

```sh
sudo systemctl enable chronyd && sudo systemctl start chronyd
```

Check the Chrony service status:

```sh
sudo systemctl status chronyd
```

For more details, refer to the official [RHEL 8 and 9 documentation](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/configuring_basic_system_settings/configuring-time-synchronization_configuring-basic-system-settings).

### Amazon Linux 2

Install Chrony, a replacement for NTP:

```sh
sudo yum install chrony
```

Edit the Chrony configuration file:

```sh
sudo nano /etc/chrony.conf
```

Add `server pool.ntp.org` to the file, replace `pool.ntp.org` with your own NTP server, then save.

Enable and start the Chrony service:

```sh
sudo systemctl enable chronyd && sudo systemctl start chronyd
```

Check the Chrony service status:

```sh
sudo systemctl status chronyd
```

For more details, refer to the official [Amazon Linux 2 documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html).

If you are using Active-Active databases, you must use [Network Time Service (ntpd)](/docs/latest/operate/rs/databases/active-active/#network-time-service-ntp-or-chrony)
to synchronize OS clocks consistently across clusters to handle conflict resolution according to the OS time.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page