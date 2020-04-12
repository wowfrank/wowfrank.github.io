---
layout: post
title: Notes of Some Useful Linux Commands
date: 2019-11-05 00:00:00 +0300
description: Notes of Some Useful Linux Commands # (optional)
img: notes-of-linux-commands.jpg # Add image post (optional)
fig-caption: Notes of Some Useful Linux Commands # Add figcaption (optional)
tags: [Linux,Commands]
categories: ['Linux']
---


## timedatectl

In Ubuntu and most other Linux distributions, we can use the timedatectl command to display and set the current system’s time and timezone.

```bash
$ timedatectl
                      Local time: Wed 2019-01-23 22:45:47 UTC
                  Universal time: Wed 2019-01-23 22:45:47 UTC
                        RTC time: Wed 2019-01-23 22:45:48
                       Time zone: Etc/UTC (UTC, +0000)
	   System clock synchronized: yes
systemd-timesyncd.service active: yes
                 RTC in local TZ: no
$ cat /etc/timezone
Etc/UTC
$ timedatectl list-timezones
...
Europe/Oslo
Europe/Paris
Europe/Podgorica
Europe/Prague
Europe/Riga
Europe/Rome
Europe/Samara
...
$ sudo timedatectl set-timezone Asia/Shanghai
$ timedatectl
                      Local time: Wed 2019-01-24 00:27:43 CST
                  Universal time: Wed 2019-01-23 22:45:47 UTC
                        RTC time: Wed 2019-01-23 22:45:48
                       Time zone: Asia/Shanghai (CST, +0800)
	   System clock synchronized: yes
systemd-timesyncd.service active: yes
                 RTC in local TZ: no
```

## find

Similar to the locate command, using find also searches for files. The difference is, you use the find command to locate files within a given directory.

#### Find Files Under Home Directory

```bash
$ find /home -name tecmint.txt
/home/tecmint.txt
$ find /tmp -type f -empty 			# To find all empty files under certain path.
$ find /tmp -type d -empty			# To file all empty directories under certain path.
```

#### Find Files Using Name and Ignoring Case

```bash
$ find /home -iname tecmint.txt
./tecmint.txt
./Tecmint.txt
```

#### Find Directories Using Name

```bash
$ find / -type d -name Tecmint
/Tecmint
$ find . -type f -name "*.php"
./tecmint.php
./login.php
./index.php
```

#### Find Files Without 777 Permissions

```bash
$ find / -type f ! -perm 777
$ find . -type f -perm 0777 -print
$ find / -perm /u=r                  #Find all Read Only files.
```

#### Find Files with 777 Permissions and Chmod to 644

```bash 
$ find / -type f -perm 0777 -print -exec chmod 644 {} \;
$ find / -type d -perm 777 -print -exec chmod 755 {} \;
```

#### Find and remove Multiple File

```bash
$ find . -type f -name "tecmint.txt" -exec rm -f {} \;
$ find . -type f -name "*.txt" -exec rm -f {} \;
```

<div align="center"><div markdown='1'>
![none]({{site.baseurl}}/assets/img/notes-of-linux-commands.png)
</div></div>

## grep

The grep utility searches for lines which contain a search pattern. When we looked at the alias command, we used grep to search through the output of another program, ps . The grep command can also search the contents of files. Here we’re searching for the word “train” in all text files in the current directory.

```bash
$ grep 'word' filename 				
$ grep 'word' file1 file2 file3
$ grep 'string1 string2'  filename
$ cat otherfile | grep 'something'
$ command | grep 'something'
$ command option1 | grep 'data'
$ grep --color 'data' fileName
```

#### You can search recursively i.e. read all files under each directory for a string “192.168.1.5”

```bash
$ grep -r "192.168.1.5" /etc/
$ grep -R "192.168.1.5" /etc/
```

#### some other functionalities

```bash
$ grep -w "boo" file
$ grep -c 'word' /path/to/file
$ grep -l 'main' *.c
$ grep --color vivek /etc/passwd
```

## touch

```bash
$ touch -p /var/www/html/{laravel, zend, yii}.py 			# creates laravel.py, zend.py, and yii.py
```

## uname

You can obtain some system information regarding the Linux computer you’re working on with the uname command.

* Use the -a (all) option to see everything.
* Use the -s (kernel name) option to see the type of kernel.
* Use the -r (kernel release) option to see the kernel release.
* Use the -v (kernel version) option to see the kernel version.

```bash
$ uname -a
$ uname -s
$ uname -r
$ uname -v
```

## w

The w command lists the currently logged in users.

## diff

<div align="center"><div markdown='1'>
![none]({{site.baseurl}}/assets/img/notes-of-linux-commands-1.jpg)
</div></div>

Short for difference, the diff command compares the content of two files line by line. After analyzing the files, it will output the lines that do not match. Programmers often use this command when they need to make some program alterations instead of rewriting the entire source code.

```bash
$ diff [OPTION]... FILES
$ diff -y -W 70 alpha1.txt alpha2.txt --suppress-common-lines
```

The -y (side by side) option shows the line differences side by side. The -w (width) option lets you specify the maximum line width to use to avoid wraparound lines. The two files are called alpha1.txt and alpha2.txt in this example. The --suppress-common-lines prevents diff from listing the matching lines, letting you focus on the lines which have differences.

## shutdown

```bash
$ sudo shutdown -h +5         # this will shutdown the server after 5 mins
$ sudo shutdown -r 10:12      # this will restart the server at 10:12
```

<div align="center"><div markdown='1'>
![none]({{site.baseurl}}/assets/img/notes-of-linux-commands.webp)
</div></div>

