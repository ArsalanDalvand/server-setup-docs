---
title: "9.8. Scheduling Asynchronous Tasks:anacron"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.asynchronous-task-scheduling-anacron.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.asynchronous-task-scheduling-anacron.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "9.8. Scheduling Asynchronous Tasks:anacron"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.task-scheduling-cron-atd.en.md)The Debian Administrator's Handbook[Next](sect.quotas.en.md)

## 9.8.Â Scheduling Asynchronous Tasks: anacron

anacron is the daemon that completes cron for computers that are not on at all times. Since regular tasks are usually scheduled for the middle of the night, they will never be executed if the computer is off at that time. The purpose of anacron is to execute them, taking into account periods in which the computer is not working.
		
			Please note that anacron will frequently execute such activity a few minutes after booting the machine, which can render the computer less responsive. This is why the tasks in the /etc/anacrontab file are started with the nice command, which reduces their execution priority and thus limits their impact on the rest of the system. Beware, the format of this file is not the same as that of /etc/crontab; if you have particular needs for anacron, see the anacrontab(5) manual page.
		

BACK TO BASICS Priorities and nice

			Unix systems (and thus Linux) are multi-tasking and multi-user systems. Indeed, several processes can run in parallel, and be owned by different users: the kernel mediates access to the resources between the different processes. As a part of this task, it has a concept of priority, which allows it to favor certain processes over others, as needed. When you know that a process can run in low priority, you can indicate so by running it with nice program. The program will then have a smaller share of the CPU, and will have a smaller impact on other running processes. Of course, if no other process needs to run, the program will not be artificially held back.
		
nice works with levels of ânicenessâ: the positive levels (from 1 to 19) progressively lower the priority, while the negative levels (from -1 to -20) will increase it â but only root can use these negative levels. Unless otherwise indicated (see the nice(1) manual page), nice increases the current level by 10.
		
			If you discover that an already running task should have been started with nice it is not too late to fix it; the renice command changes the priority of an already running process, in either direction (but reducing the ânicenessâ of a process is reserved for the root user).
		
			Installation of the anacron package deactivates execution by cron of the scripts in the /etc/cron.hourly/, /etc/cron.daily/, /etc/cron.weekly/, and /etc/cron.monthly/ directories. This avoids their double execution by anacron and cron. The cron command remains active and will continue to handle the other scheduled tasks (especially those scheduled by users).
		[Prev9.7.Â Scheduling Tasks with cron and atd](sect.task-scheduling-cron-atd.en.md)[Up](#)[Home](index.en.md)[Next9.9.Â Quotas](sect.quotas.en.md)