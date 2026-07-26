---
title: "B.5.4. Libraries"
source: "https://www.debian.org/doc/manuals/debian-handbook/sect.user-space.en.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "debian-handbook/sect.user-space.en"
category: "debian-linux"
retrieved_at: "2026-07-26T15:27:59+00:00"
tags:
  - debian
  - linux-sysadmin
  - debian-handbook
aliases:
  - "B.5.4. Libraries"
---
[Download the ebook](http://debian-handbook.info/get/)

[](http://www.debian.org)[](index.en.md)

[Prev](sect.kernel-role-and-tasks.en.md)The Debian Administrator's Handbook

## B.5.Â The User Space

			âUser spaceâ refers to the runtime environment of normal (as opposed to kernel) processes. This does not necessarily mean these processes are actually started by users because a standard system normally has several âdaemonâ (or background) processes running before the user even opens a session. Daemon processes are also considered user-space processes.
		

### B.5.1.Â Process

				When the kernel gets past its initialization phase, it starts the very first process, init. Process #1 alone is very rarely useful by itself, and Unix-like systems run with many additional processes.
			
				First of all, a process can clone itself (this is known as a fork). The kernel allocates a new (but identical) process memory space, and another process to use it. At this time, the only difference between these two processes is their pid. The new process is usually called a child process, and the original process whose pid doesn't change, is called the parent process.
			
				Sometimes, the child process continues to lead its own life independently from its parent, with its own data copied from the parent process. In many cases, though, this child process executes another program. With a few exceptions, its memory is simply replaced by that of the new program, and execution of this new program begins. This is the mechanism used by the init process (with process number 1) to start additional services and execute the whole startup sequence. At some point, one process among init's offspring starts a graphical interface for users to log in to (the actual sequence of events is described in more details in [SectionÂ 9.1, âSystem Bootâ](unix-services.en.md#sect.system-boot)).
			
				When a process finishes the task for which it was started, it terminates. The kernel then recovers the memory assigned to this process, and stops giving it slices of running time. The parent process is told about its child process being terminated, which allows a process to wait for the completion of a task it delegated to a child process. This behavior is plainly visible in command-line interpreters (known as shells). When a command is typed into a shell, the prompt only comes back when the execution of the command is over. Most shells allow for running the command in the background, it is a simple matter of adding an & to the end of the command. The prompt is displayed again right away, which can lead to problems if the command needs to display data of its own.
			

### B.5.2.Â Daemons

				A âdaemonâ is a process started automatically by the boot sequence. It keeps running (in the background) to perform maintenance tasks or provide services to other processes. This âbackground taskâ is actually arbitrary, and does not match anything particular from the system's point of view. They are simply processes, quite similar to other processes, which run in turn when their time slice comes. The distinction is only in the human language: a process that runs with no interaction with a user (in particular, without any graphical interface) is said to be running âin the backgroundâ or âas a daemonâ.
			

VOCABULARY Daemon, demon, a derogatory term?

				Although daemon term shares its Greek etymology with demon, the former does not imply diabolical evil, instead, it should be understood as a kind of helper spirit. This distinction is subtle enough in English; it is even worse in other languages where the same word is used for both meanings.
			
				Several such daemons are described in detail in [ChapterÂ 9, Unix Services](unix-services.en.md).
			

### B.5.3.Â Inter-Process Communications

				An isolated process, whether a daemon or an interactive application, is rarely useful on its own, which is why there are several methods allowing separate processes to communicate together, either to exchange data or to control one another. The generic term referring to this is inter-process communication, or IPC for short.
			
				The simplest IPC system is to use files. The process that wishes to send data writes it into a file (with a name known in advance), while the recipient only has to open the file and read its contents.
			
				In the case where you do not wish to store data on disk, you can use a pipe, which is simply an object with two ends; bytes written in one end are readable at the other. If the ends are controlled by separate processes, this leads to a simple and convenient inter-process communication channel. Pipes can be classified into two categories: named pipes, and anonymous pipes. A named pipe is represented by an entry on the filesystem (although the transmitted data is not stored there), so both processes can open it independently if the location of the named pipe is known beforehand. In cases where the communicating processes are related (for instance, a parent and its child process), the parent process can also create an anonymous pipe before forking, and the child inherits it. Both processes will then be able to exchange data through the pipe without needing the filesystem.
			

IN PRACTICE A concrete example

				Let's describe in some detail what happens when a complex command (a pipeline) is run from a shell. We assume we have a bash process (the standard user shell on Debian), with pidÂ 4374; into this shell, we type the command: ls | sort .
			
				The shell first interprets the command typed in. In our case, it understands there are two programs (ls and sort), with a data stream flowing from one to the other (denoted by the | character, known as pipe). bash first creates an unnamed pipe (which initially exists only within the bash process itself).
			
				Then the shell clones itself; this leads to a new bash process, with pidÂ #4521 (pids are abstract numbers, and generally have no particular meaning). Process #4521 inherits the pipe, which means it is able to write in its âinputâ side; bash redirects its standard output stream to this pipe's input. Then it executes (and replaces itself with) the ls program, which lists the contents of the current directory. Since ls writes on its standard output, and this output has previously been redirected, the results are effectively sent into the pipe.
			
				A similar operation happens for the second command: bash clones itself again, leading to a new bash process with pid #4522. Since it is also a child process of #4374, it also inherits the pipe; bash then connects its standard input to the pipe output, then executes (and replaces itself with) the sort command, which sorts its input and displays the results.
			
				All the pieces of the puzzle are now set up: ls reads the current directory and writes the list of files into the pipe; sort reads this list, sorts it alphabetically, and displays the results. Processes numbers #4521 and #4522 then terminate, and #4374 (which was waiting for them during the operation), resumes control and displays the prompt to allow the user to type in a new command.
			
				Not all inter-process communications are used to move data around, though. In many situations, the only information that needs to be transmitted are control messages such as âpause executionâ or âresume executionâ. Unix (and Linux) provides a mechanism known as signals, through which a process can simply send a specific signal (chosen from a predefined list of signals) to another process. The only requirement is to know the pid of the target.
			
				For more complex communications, there are also mechanisms allowing a process to open access, or share, part of its allocated memory to other processes. Memory now shared between them can be used to move data between the processes.
			
				Finally, network connections can also help processes communicate; these processes can even be running on different computers, possibly thousands of kilometers apart.
			
				It is quite standard for a typical Unix-like system to make use of all these mechanisms to various degrees.
			

### B.5.4.Â Libraries

				Function libraries play a crucial role in a Unix-like operating system. They are not proper programs, since they cannot be executed on their own, but collections of code fragments that can be used by standard programs. Among the common libraries, you can find:
			
						the standard C library (glibc), which contains basic functions such as ones to open files or network connections, and others facilitating interactions with the kernel;
					
						graphical toolkits, such as Gtk+ and Qt, allowing many programs to reuse the graphical objects they provide;
					
						the libpng library, which allows loading, interpreting and saving images in the PNG format.
					
				Thanks to those libraries, applications can reuse existing code. Application development is simplified since many applications can reuse the same functions. With libraries often developed by different persons, the global development of the system is closer to Unix's historical philosophy.
			

CULTURE The Unix Way: one thing at a time

				One of the fundamental concepts that underlies the Unix family of operating systems is that each tool should only do one thing, and do it well; applications can then reuse these tools to build more advanced logic on top. This philosophy can be seen in many incarnations. Shell scripts may be the best example: they assemble complex sequences of very simple tools (such as grep, wc, sort, uniq and so on). Another implementation of this philosophy can be seen in code libraries: the libpng library allows reading and writing PNG images, with different options and in different ways, but it does only that; no question of including functions that display or edit images.
			
				Moreover, these libraries are often referred to as âshared librariesâ, since the kernel is able to only load them into memory once, even if several processes use the same library at the same time. This allows saving memory, when compared with the opposite (hypothetical) situation where the code for a library would be loaded as many times as there are processes using it.
			[PrevB.4.Â Some Tasks Handled by the Kernel](sect.kernel-role-and-tasks.en.md)[Up](#)[Home](index.en.md)