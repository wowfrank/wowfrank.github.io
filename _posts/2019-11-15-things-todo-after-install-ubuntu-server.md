---
layout: post
title: "安装Ubuntu20.04后的习惯性配置记录"
date: 2019-11-15 00:00:00 +0800
description: "安装Ubuntu20.04后的习惯性配置记录" # (optional)
img: Setup-a-Python-Virtual-Environment-on-Ubuntu.jpg # Add image post (optional)
fig-caption: "安装Ubuntu20.04后的习惯性配置记录" # Add figcaption (optional)
tags: ['Linux','Ubuntu','Commands']
categories: ['Linux']
---

## 1. 配置APT的源，改为阿里源

```bash
$ lsb_release -a
$ cd /etc/apt
$ sudo mv sources.list sources.list.bak
$ sudo vi sources.list
	deb http://mirrors.aliyun.com/ubuntu/ focal main multiverse restricted universe
	deb http://mirrors.aliyun.com/ubuntu/ focal-backports main multiverse restricted universe
	deb http://mirrors.aliyun.com/ubuntu/ focal-proposed main multiverse restricted universe
	deb http://mirrors.aliyun.com/ubuntu/ focal-security main multiverse restricted universe
	deb http://mirrors.aliyun.com/ubuntu/ focal-updates main multiverse restricted universe
	deb-src http://mirrors.aliyun.com/ubuntu/ focal main multiverse restricted universe
	deb-src http://mirrors.aliyun.com/ubuntu/ focal-backports main multiverse restricted universe
	deb-src http://mirrors.aliyun.com/ubuntu/ focal-proposed main multiverse restricted universe
	deb-src http://mirrors.aliyun.com/ubuntu/ focal-security main multiverse restricted universe
	deb-src http://mirrors.aliyun.com/ubuntu/ focal-updates main multiverse restricted universe
$ sudo apt update
```

## 2. 配置VI

```bash
" Set compatibility to Vim only.
set nocompatible

" Helps force plug-ins to load correctly when it is turned back on below.
filetype off

" Turn on syntax highlighting.
"syntax on

" For plug-ins to load correctly.
filetype plugin indent on

" Turn off modelines
set modelines=0

" Automatically wrap text that extends beyond the screen length.
set wrap
" Vim's auto indentation feature does not work properly with text copied from outside of Vim. Press the <F2> key to toggle paste mode on/off.
nnoremap <F2> :set invpaste paste?<CR>
imap <F2> <C-O>:set invpaste paste?<CR>
set pastetoggle=<F2>

" Uncomment below to set the max textwidth. Use a value corresponding to the width of your screen.
 set textwidth=79
set formatoptions=tcqrn1
set tabstop=4
set shiftwidth=4
set softtabstop=4
set expandtab
set noshiftround

" Display 5 lines above/below the cursor when scrolling with a mouse.
set scrolloff=5
" Fixes common backspace problems
set backspace=indent,eol,start

" Speed up scrolling in Vim
set ttyfast

" Status bar
set laststatus=2

" Display options
set showmode
set showcmd

" Highlight matching pairs of brackets. Use the '%' character to jump between them.
set matchpairs+=<:>

" Display different types of white spaces.
" set list
" set listchars=tab:›\ ,trail:•,extends:#,nbsp:.

" Show line numbers
set number

" Set status line display
set statusline=%F%m%r%h%w\ [FORMAT=%{&ff}]\ [TYPE=%Y]\ [POS=%l,%v][%p%%]\ [BUFFER=%n]\ %{strftime('%c')}

" Encoding
set encoding=utf-8

" Highlight matching search patterns
set hlsearch
" Enable incremental search
set incsearch
" Include matching uppercase words with lowercase search term
set ignorecase
" Include only uppercase words with uppercase search term
set smartcase

" Store info from no more than 100 files at a time, 9999 lines of text, 100kb of data. Useful for copying large amounts of data between files.
set viminfo='100,<9999,s100

" Map the <Space> key to toggle a selected fold opened/closed.
nnoremap <silent> <Space> @=(foldlevel('.')?'za':"\<Space>")<CR>
vnoremap <Space> zf

" Automatically save and load folds
autocmd BufWinLeave *.* mkview
autocmd BufWinEnter *.* silent loadview"
```

## 3. 安装SAMBA

```bash
$ sudo apt -y install samba
$ sudo mkdir -p /var/www/html/share
$ sudo chmod -R 0777 /var/www/html/share
$ sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
$ sudo vi /etc/samba/smb.conf

	# line 25: add
	unix charset = UTF-8
	# line 30: change (Windows' default)
	workgroup = WORKGROUP
	# line 40: uncomment and change IP address you allow
	interfaces = 127.0.0.0/8 eth0
	# line 48: uncomment and add
	bind interfaces only = yes
	map to guest = Bad User
	# add to the end
	# any share name you like
	[Share]
		# comment of this directory
		comment = just a shared folder for other devices
	    # shared directory
	    path = /var/www/html/share
	    # writable
	    writable = yes
	    # guest OK
	    guest ok = yes
	    # guest only
	    guest only = yes
	    read only = no
	    browsable = yes
	    # fully accessed
	    create mode = 0777
	    # fully accessed
	    directory mode = 0777

	    force user = frank
	    force group = frank

$ sudo systemctl restart smbd
```

## 4. 安装Python3的PIP

```bash
$ sudo apt install python3-pip
```

## 5. 安装UFW

```bash
$ sudo apt update
$ sudo apt install ufw
$ sudo ufw status
$ sudo ufw allow ssh
$ sudo ufw allow from 192.168.1.0/24 to any
$ sudo ufw enable
$ sudo ufw status verbose
```

## 6. 安装LEMP

```bash
$ sudo apt install nginx
$ sudo systemctl enable nginx
$ sudo systemctl start nginx
$ sudo systemctl status nginx
$ nginx -v
$ sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT
$ sudo ufw allow http
$ sudo ufw status verbose
$ sudo apt install mariadb-server mariadb-client
$ sudo systemctl start mariadb
$ sudo systemctl enable mariadb
$ sudo mysql_secure_installation

MariaDB[(none)]> CREATE DATABASE `laravel`;
MariaDB[(none)]> GRANT ALL ON `laravel`.* TO 'db_user'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
MariaDB[(none)]> FLUSH PRIVILEGES;
MariaDB[(none)]> exit;

$ mariadb -u db_user -p
$ sudo apt install  -y php php-fpm php-mysql php-common php-cli php7.4-common \
	php7.4-opcache php-readline php-mbstring php-xml php-gd php-curl \
	php-json php-zip php-pear
$ sudo systemctl start php7.4-fpm
$ sudo systemctl enable php7.4-fpm
$ systemctl status php7.4-fpm
# configure nginx default configuration
$ sudo nginx -t
$ sudo ln -s /etc/nginx/sites-available/xxx.conf /etc/nginx/sites-enabled/xxx.conf
$ sudo systemctl reload nginx
```

## 配置Python Virtual Environment 

原文出处：[How to Set Up a Python Virtual Environment on Debian 10 Buster](https://linuxconfig.org/how-to-set-up-a-python-virtual-environment-on-debian-10-buster){:target="_blank"}

```bash
$ sudo apt install python3 python3-venv
$ sudo apt install virtualenv python3-virtualenv

# USE PYTHON3's VENV
$ sudo mkdir -p /var/www/html/vt-env/
$ sudo chown -R user:group /var/www/html/vt-env/
$ cd /var/www/html/vt-env/
# Now, you're working with the Python install from your virtual environment, instead of the system wide one. Anything you do now, should reside in your project folder. When you're done, just run deactivate to exit the virtual Python.
$ python3 -m venv xxx-project
$ source xxx-project/bin/activate

# USE VIRTUALENV
# To start, create your environment with the virtualenv command. You'll also need to tell it to use Python 3 with the -p flag.
$ virtualenv -p python3 vtEnvProject
$ source vtEnvProject/bin/activate

$ python -m pip install Django
or
$ git clone https://github.com/django/django.git
$ python -m pip install -e django/

# Install Numpy
$ pip3 install --index-url https://pypi.douban.com/simple/ numpy

# Do your work inside the project directories. When you're done, use deactivate to exit the virtual environment.
$ deactivate
```
