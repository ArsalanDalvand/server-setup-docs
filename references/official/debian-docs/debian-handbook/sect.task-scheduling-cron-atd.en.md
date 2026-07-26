---
title: "9.7.2. Using theatCommand"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.task-scheduling-cron-atd.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.task-scheduling-cron-atd.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.7.2. Using theatCommand"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.inetd.en.md)The Debian Administrator's Handbook[Next](sect.asynchronous-task-scheduling-anacron.en.md)

## 9.7.Â Scheduling Tasks with cron and atd

cron is the daemon responsible for executing scheduled and recurring commands (every hour, every day, every week, etc.). atd deals with commands to be executed a single time, but at a specific moment in the future.
		
			In a Unix system, many tasks are scheduled for regular execution:
		
					rotating the logs;
				
					updating the database for the locate program;
				
					back-ups;
				
					maintenance scripts (such as cleaning out temporary files).
				
			By default, all users can schedule the execution of tasks. Each user has thus their own crontab in which they can record scheduled commands. It can be edited by running crontab -e (its content is stored in the /var/spool/cron/crontabs/user file).
		

SECURITY Restricting cron or atd

			You can restrict access to crontab by creating an explicit authorization file (whitelist) in /etc/cron.allow, in which you indicate the only users authorized to schedule commands. All others will automatically be deprived of this feature. Conversely, to only block one or two troublemakers, you could write their username in the explicit prohibition file (blacklist), /etc/cron.deny. This same feature is available for atd, with the /etc/at.allow and /etc/at.deny files (also described in their manual pages).
		
			The root user has their own crontab, but can also use the /etc/crontab file, or write additional crontab files in the /etc/cron.d directory. These last two solutions have the advantage of being able to specify the user identity to use when executing the command.
		
			The cron package includes by default some scheduled commands that execute:
		
					programs in the /etc/cron.hourly/ directory once per hour;
				
					programs in /etc/cron.daily/ once per day;
				
					programs in /etc/cron.weekly/ once per week;
				
					programs in /etc/cron.monthly/ once per month.
				
			Many Debian packages rely on this service: by putting maintenance scripts in these directories, they ensure optimal operation of their services.
		

### 9.7.1.Â Format of a crontab File

TIP Text shortcuts for cron

cron recognizes some abbreviations which replace the first five fields in a crontab entry. They correspond to the most classic scheduling options:
			
@yearly: once per year (January 1, at 00:00);
					
@monthly: once per month (the 1st of the month, at 00:00);
					
@weekly: once per week (Sunday at 00:00);
					
@daily: once per day (at 00:00);
					
@hourly: once per hour (at the beginning of each hour).
					
				More abbreviations are recognized, like @rebootor @midnight.
			

SPECIAL CASE cron and daylight savings time

				In Debian, cron takes the time change (for Daylight Savings Time, or in fact for any significant change in the local time) into account as best as it can. Thus, the commands that should have been executed during an hour that never existed (for example, tasks scheduled at 2:30 am during the Spring time change in France, since at 2:00 am the clock jumps directly to 3:00 am) are executed shortly after the time change (thus around 3:00 am DST). On the other hand, in autumn, when commands would be executed several times (2:30 am DST, then an hour later at 2:30 am standard time, since at 3:00 am DST the clock turns back to 2:00 am) are only executed once.
			
				Be careful, however, if the order in which the different scheduled tasks and the delay between their respective executions matters, you should check the compatibility of these constraints with cron's behavior; if necessary, you can prepare a special schedule for the two problematic nights per year.
			
				Each significant line of a crontab entry describes a scheduled command with the six (or seven) following fields:
			
						the value for the minute (from 0 to 59);
					
						the value for the hour (from 0 to 23);
					
						the value for the day of the month (from 1 to 31);
					
						the value for the month (from 1 to 12);
					
						the value for the day of the week (from 0 to 7, 1 corresponding to Monday, Sunday being represented by both 0 and 7; it is also possible to use the first three letters of the name of the day of the week in English, such as Sun, Mon, etc.);
					
						the user name under whose identity the command must be executed (in the /etc/crontab file and in the fragments located in /etc/cron.d/, but not in the users' own crontab files);
					
						the command to execute (when the conditions defined by the first five columns are met).
					
				All these details are documented in the crontab(5) man page.
			
				Each value can also be expressed in the form of a list of possible values (separated by commas). The syntax a-b describes the interval of all the values between a and b. The syntax a-b/c describes the interval with an increment of c (example: 0-10/2 means 0,2,4,6,8,10). An asterisk * is a wildcard, representing all possible values.
			

ExampleÂ 9.2.Â Sample user crontab file

```bash
#Format
#min hour day mon dow  command

# Download data every night at 7:25 pm
 25  19   *   *   *    $HOME/bin/get.pl

# 8:00 am, on weekdays (Monday through Friday)
 00  08   *   *   1-5  $HOME/bin/dosomething

# every two hours
 *  */2   *   *   *    $HOME/bin/dosomethingelse

# Restart the IRC proxy after each reboot
@reboot /usr/bin/dircproxy
```

TIP Executing a command on boot

				To execute a command a single time, just after booting the computer, you can use the @reboot macro (a simple restart of cron does not trigger a command scheduled with @reboot). This macro replaces the first five fields of an entry in the crontab.
			

ALTERNATIVE Emulating cron with systemd

				It is possible to emulate part of cron's behavior with systemd's timer mechanism (see [SectionÂ 9.1.1, âThe systemd init systemâ](unix-services.en.md#sect.systemd)).
			

### 9.7.2.Â Using the at Command

				The at executes a command at a specified moment in the future. It takes the desired time and date as command-line parameters, and the command to be executed in its standard input. The command will be executed as if it had been entered in the current shell. at even takes care to retain the current environment, in order to reproduce the same conditions when it executes the command. The time is indicated by following the usual conventions: 16:12 or 4:12pm represents 4:12 pm. The date can be specified in several European and Western formats, including DD.MM.YY (27.07.22 thus representing 27 July 2022), YYYY-MM-DD (this same date being expressed as 2022-07-27), MM/DD/[CC]YY (i.e., 12/25/22 or 12/25/2022 will be December 25, 2022), or simple MMDD[CC]YY (so that 122522 or 12252022 will, likewise, represent December 25, 2022). Without it, the command will be executed as soon as the clock reaches the time indicated (the same day, or tomorrow if that time has already passed on the same day). You can also simply write âtodayâ or âtomorrowâ, which is self-explanatory.
			

```bash
$ at 09:00 27.07.22 <<END
> echo "Don't forget to wish a Happy Birthday to RaphaÃ«l!" \
>   | mail lolando@debian.org
> END
warning: commands will be executed using /bin/sh
job 1 at Wed Jul 27 09:00:00 2022
```

				An alternative syntax postpones the execution for a given duration: at now + number period. The period can be minutes, hours, days, or weeks. The number simply indicates the number of said units that must elapse before execution of the command.
			
				To cancel a task scheduled by cron, simply run crontab -e and delete the corresponding line in the crontab file. For at tasks, it is almost as easy: run atrm task-number. The task number is indicated by the at command when you scheduled it, but you can find it again with the atq command, which gives the current list of scheduled tasks.
			[Prev9.6.Â The inetd Super-Server](sect.inetd.en.md)[Up](#)[Home](index.en.md)[Next9.8.Â Scheduling Asynchronous Tasks: anacron](sect.asynchronous-task-scheduling-anacron.en.md)