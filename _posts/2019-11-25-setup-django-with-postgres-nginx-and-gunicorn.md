---
layout: post
title: "Notes of Set Up Django with Postgres, Nginx, and Gunicorn"
date: 2019-11-15 00:00:00 +0800   # +0800 --> Asia/Shanghai Timezone
description: "Set Up Django with Postgres, Nginx, and Gunicorn" # (optional)
img: django-postgres-nginx-gunicorn.png # Add image post (optional)
fig-caption: "Notes of Some Useful Linux Commands" # Add figcaption (optional)
tags: ['Ubuntu','Django','Postgres', 'Nginx', 'Gunicorn']
categories: ['Linux']
---

## Install the Packages

```bash
$ sudo apt update
$ sudo apt install python3-pip python3-dev libpq-dev postgresql postgresql-contrib nginx curl
```

## Creating the PostgreSQL Database and User

```bash
$ sudo -u postgres psql
postgres=# CREATE DATABASE myproject;
postgres=# CREATE USER myprojectuser WITH PASSWORD 'password';
postgres=# ALTER ROLE myprojectuser SET client_encoding TO 'utf8';
postgres=# ALTER ROLE myprojectuser SET default_transaction_isolation TO 'read committed';
postgres=# ALTER ROLE myprojectuser SET timezone TO 'UTC';
postgres=# GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser;
postgres=# \q
$ sudo systemctl status postgresql
$ sudo systemctl start postgresql
$ sudo systemctl enable postgresql
```

## Creating a Python Virtual Environment for your Project

```bash
$ sudo -H pip3 install --upgrade pip
$ sudo -H pip3 install virtualenv
$ mkdir ~/myprojectdir
$ cd ~/myprojectdir
$ virtualenv myprojectenv
$ source myprojectenv/bin/activate
# Note: When the virtual environment is activated (when your prompt has (myprojectenv) preceding it), use pip instead of pip3, even if you are using Python 3. The virtual environment’s copy of the tool is always named pip, regardless of the Python version.
(myprojectenv) $ pip install django gunicorn psycopg2-binary
```

## Creating and Configuring a New Django Project



```bash
(myprojectenv) $ django-admin.py startproject myproject ~/myprojectdir
(myprojectenv) $ nano ~/myprojectdir/myproject/settings.py
. . .
# ~/myprojectdir/myproject/settings.py
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'myproject',
        'USER': 'myprojectuser',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORT': '',
    }
}
. . .
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'static/')
. . .
(myprojectenv) $ ~/myprojectdir/manage.py makemigrations
(myprojectenv) $ ~/myprojectdir/manage.py migrate
(myprojectenv) $ ~/myprojectdir/manage.py createsuperuser
(myprojectenv) $ sudo ufw allow 8000
(myprojectenv) $ ~/myprojectdir/manage.py runserver 0.0.0.0:8000
(myprojectenv) $ cd ~/myprojectdir
(myprojectenv) $ gunicorn --bind 0.0.0.0:8000 myproject.wsgi
(myprojectenv) $ deactivate
```

## Creating systemd Socket and Service Files for Gunicorn

```bash
$ sudo nano /etc/systemd/system/gunicorn.socket
. . .
[Unit]
Description=gunicorn socket

[Socket]
ListenStream=/run/gunicorn.sock

[Install]
WantedBy=sockets.target
. . .
$ sudo nano /etc/systemd/system/gunicorn.service
. . .
[Unit]
Description=gunicorn daemon
Requires=gunicorn.socket
After=network.target

[Service]
User=sammy
Group=www-data
WorkingDirectory=/home/sammy/myprojectdir
ExecStart=/home/sammy/myprojectdir/myprojectenv/bin/gunicorn \
          --access-logfile - \
          --workers 3 \
          --bind unix:/run/gunicorn.sock \
          myproject.wsgi:application

[Install]
WantedBy=multi-user.target
. . .
$ sudo systemctl start gunicorn.socket
$ sudo systemctl enable gunicorn.socket
```

## Checking for the Gunicorn Socket File

```bash
$ sudo systemctl status gunicorn.socket
$ file /run/gunicorn.sock
$ sudo journalctl -u gunicorn.socket
```

## Testing Socket Activation

```bash
$ sudo systemctl status gunicorn
$ sudo journalctl -u gunicorn
$ sudo systemctl daemon-reload
$ sudo systemctl restart gunicorn
```

## Configure Nginx to Proxy Pass to Gunicorn

```bash
$ sudo nano /etc/nginx/sites-available/myproject
. . .
server {
    listen 80;
    server_name server_domain_or_IP;

    location = /favicon.ico { access_log off; log_not_found off; }
    location /static/ {
        root /home/sammy/myprojectdir;
    }

    location / {
        include proxy_params;
        proxy_pass http://unix:/run/gunicorn.sock;
    }
}
. . .
$ sudo ln -s /etc/nginx/sites-available/myproject /etc/nginx/sites-enabled
$ sudo nginx -t
$ sudo systemctl restart nginx
```