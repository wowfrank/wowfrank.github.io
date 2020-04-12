---
layout: post
title: Running Linux Commands to Bring Jobs Background and Foreground
date: 2019-09-08 00:00:00 +0300
description: Running Linux Commands to Bring Jobs Background and Foreground # Add post description (optional)
img: comands-background-foregound.jpg # Add image post (optional)
fig-caption: none # Add figcaption (optional)
tags: [commands,linux,backgound,foregound,process]
categores: [Linux]
---

Typically when you run a command in the terminal, you have to wait until the command finishes before you can enter another one. This is called running the command in the foreground or foreground process. When a process runs in the foreground, it occupies your shell, and you can interact with it using the input devices.

What if the command takes a long time to finish, and you want to run other commands in the meantime? You have several options at your disposal. The most obvious and straightforward option is to start a new shell session and run the command in it. Another option is to run the command in the background.

A background process is a process/command that is started from a terminal and runs in the background, without interaction from the user.

In this article, we will talk about the background processes is Linux. We will show you how to start a command in the background and how to keep the process running after the shell session is closed.

## **Run a Linux Command in the Background**

To run a command in the background, add the ampersand symbol (&) at the end of the command:

```bash
somebody@some-host:~$ some_command &
somebody@some-host:~$ sleep 60 &
```

The shell job ID (surrounded with brackets) and process ID will be printed on the terminal:

```bash
somebody@some-host:~$ sleep 60 &
[1] 8888
```

You can have multiple processes running in the background at the same time.

The background process will continue to write messages to the terminal from which you invoked the command. To suppress the stdout and stderr messages use the following syntax:

```bash
somebody@some-host:~$ sleep 60  > /dev/null 2>&1 & 
[1] 8989
```

&gt;/dev/null 2>&1 means redirect stdout to /dev/null and stderr to stdout.

Use the jobs utility to display the status of all stopped and background jobs in the current shell session:

```bash
somebody@some-host:~$ jobs -l
[1]- 8888 Running    		sleep 60 &
[2]+ 8899 Running    		sleep 60 > /dev/null 2>&1 & 
```

To bring a background process to the foreground, use the fg command, or you can also use fg %#:

```bash
somebody@some-host:~$ fg
sleep 60 > /dev/null 2>&1 & 
^Z [CTRL-Z]
[2]+  Stopped                 sleep 60 > /dev/null 2>&1
somebody@some-host:~$ fg %1
sleep 60 &
^Z [CTRL-Z]
[1]- Stopped                 sleep 60 &
somebody@some-host:~$
```

To terminate the background process, use the kill command followed by the process ID:

```bash
somebody@some-host:~$ ps -f
UID        PID  PPID  C STIME TTY          TIME CMD
someone   2803  2802  0 09:20 pts/0    00:00:00 -bash
someone   8587  2803  0 15:47 pts/0    00:00:00 sleep 60
someone   8589  2803  0 15:47 pts/0    00:00:00 sleep 60
someone   8590  2803  1 15:47 pts/0    00:00:00 ps -f
somebody@some-host:~$ kill -9 8587
```

### **That’s it**

This was a quick one but enough for you to learn a few things about running commands in background in Linux. I would advise learning nohup command as well. This command lets you run commands in background even after you log out of the session.