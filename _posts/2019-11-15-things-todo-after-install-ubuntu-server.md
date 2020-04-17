---
layout: post
title: "Things To Do After Install Ubuntu Server 19.10"
date: 2019-11-15 00:00:00 +0800
description: "Things To Do After Install Ubuntu Server 19.10" # (optional)
img: notes-of-linux-commands.jpg # Add image post (optional)
fig-caption: "Things To Do After Install Ubuntu Server 19.10" # Add figcaption (optional)
tags: ['Linux','Ubuntu','Commands']
categories: ['Linux']
---

## 1. Configure repository of apt

```bash
$ lsb_release -a
$ cd /etc/apt
$ sudo mv sources.list sources.list.bak
$ sudo vi sources.list
	deb http://mirrors.aliyun.com/ubuntu/ eoan main multiverse restricted universe
	deb http://mirrors.aliyun.com/ubuntu/ eoan-backports main multiverse restricted universe
	deb http://mirrors.aliyun.com/ubuntu/ eoan-proposed main multiverse restricted universe
	deb http://mirrors.aliyun.com/ubuntu/ eoan-security main multiverse restricted universe
	deb http://mirrors.aliyun.com/ubuntu/ eoan-updates main multiverse restricted universe
	deb-src http://mirrors.aliyun.com/ubuntu/ eoan main multiverse restricted universe
	deb-src http://mirrors.aliyun.com/ubuntu/ eoan-backports main multiverse restricted universe
	deb-src http://mirrors.aliyun.com/ubuntu/ eoan-proposed main multiverse restricted universe
	deb-src http://mirrors.aliyun.com/ubuntu/ eoan-security main multiverse restricted universe
	deb-src http://mirrors.aliyun.com/ubuntu/ eoan-updates main multiverse restricted universe
$ sudo apt update
```

## 2. Configure vim