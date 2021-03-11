---
layout: post
title: "Examples of Docker Compose Config"
date: 2020-12-11 01:01:00 +0800
description: "Examples of Docker Compose Config" # (optional)
img: 2020-12-11-cover-image-of-docker-compose-config.jpg # Add image post (optional)
fig-caption: "Examples of Docker Compose Config" # Add figcaption (optional)
tags: ['Programming', 'Docker']
categories: ['Programming', 'Docker']
---

## Docker Compose YML of Golang

### yml config file

```yaml
version: '3.9'

networks:
    shared:
        external:
            name: local-bridge-net
        
services:
    app:
        image: golang:latest
        container_name: golang_app
        working_dir: /go/src/
        build:
            context: ./go
        volumes:
            - ./go/:/go/
        ports:
            - "8000:8080"
        environment:
            WORKING_DIR: /go/src/
        # command: go run /go/src/example.com/http_demo/main.go
        tty: true 
        networks:
            - shared
```

### Dockerfile

```sh
FROM golang:latest

RUN apt-get update
RUN apt-get install vim -y
RUN go get "github.com/go-sql-driver/mysql"
```

## Docker Compose YML of Laravel

### yml config file

```yml
version: '3.9'

networks:
    shared:
        external:
            name: local-bridge-net

services:
    site:
        build:
            context: .
            dockerfile: nginx.dockerfile
        container_name: nginx_shared
        ports:
            - "8088:80"
        # expose:
        #   # Opens port 3306 on the container
        #   - 8088
        volumes:
            - ./src:/var/www/html:delegated
        depends_on:
            - php
        networks:
            - shared

    php:
        build:
            context: .
            dockerfile: php.dockerfile
        container_name: php
        volumes:
            - ./src:/var/www/html
        ports:
            - "9000:9000"
        networks:
            - shared
    # npm:
    #     build:
    #         context: .
    #         dockerfile: node.dockerfile
    #     container_name: node_shared
    #     tty: true
    #     ports:
    #         - 8080:8080
    #     volumes:
    #         - ./src:/var/www/html
    #     # volumes:
    #     #  - ./src:/var/www/html
    #     # command: npm run serve
    #     networks:
    #         - shared

    # composer:
    #     build:
    #         context: .
    #         dockerfile: composer.dockerfile
    #     container_name: composer
    #     volumes:
    #         - ./src:/var/www/html
    #     working_dir: /var/www/html
    #     depends_on:
    #         - php
    #     user: laravel
    #     networks:
    #         - shared
    #     entrypoint: ['composer', '--ignore-platform-reqs']

    # npm:
    #     image: node
    #     container_name: npm
    #     volumes:
    #       - ./src:/var/www/html
    #     working_dir: /var/www/html
    #     entrypoint: ['npm']

    # artisan:
    #     build:
    #         context: .
    #         dockerfile: php.dockerfile
    #     container_name: artisan
    #     volumes:
    #         - ./src:/var/www/html:delegated
    #     working_dir: /var/www/html
    #     user: laravel
    #     entrypoint: ['php', '/var/www/html/artisan']
    #     networks:
    #         - shared
```

### composer.dockerfile

```sh
FROM composer:latest

RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel

WORKDIR /var/www/html
```

### nginx.dockerfile

```sh
FROM nginx:stable-alpine

ADD ./nginx/nginx.conf /etc/nginx/nginx.conf
ADD ./nginx/default.conf /etc/nginx/conf.d/default.conf

RUN mkdir -p /var/www/html

RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel

RUN chown -R laravel:laravel /var/www/html
```


### php.dockerfile

```sh
FROM php:7.4-fpm-alpine

ADD ./php/www.conf /usr/local/etc/php-fpm.d/www.conf

RUN addgroup -g 1000 laravel && adduser -G laravel -g laravel -s /bin/sh -D laravel

RUN mkdir -p /var/www/html

RUN chown laravel:laravel /var/www/html

WORKDIR /var/www/html

RUN docker-php-ext-install pdo pdo_mysql exif

# Install essential build tools
RUN apk add --no-cache \
    git \
    yarn \
    autoconf \
    g++ \
    make \
    openssl-dev

# Optional, force UTC as server time
RUN echo "UTC" > /etc/timezone

# Install composer
ENV COMPOSER_HOME /composer
ENV PATH ./vendor/bin:/composer/vendor/bin:$PATH
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

# Setup bzip2 extension
RUN apk add --no-cache \
    bzip2-dev \
    && docker-php-ext-install -j$(nproc) bz2 \
    && docker-php-ext-enable bz2 \
    && rm -rf /tmp/*

# Setup GD extension
RUN apk add --no-cache \
      freetype \
      libjpeg-turbo \
      libpng \
      freetype-dev \
      libjpeg-turbo-dev \
      libpng-dev \
    && docker-php-ext-configure gd \
      --with-freetype=/usr/include/ \
      # --with-png=/usr/include/ \ # No longer necessary as of 7.4; https://github.com/docker-library/php/pull/910#issuecomment-559383597
      --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-enable gd \
    && apk del --no-cache \
      freetype-dev \
      libjpeg-turbo-dev \
      libpng-dev \
    && rm -rf /tmp/*

# Install intl extension
RUN apk add --no-cache \
    icu-dev \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-enable intl \
    && rm -rf /tmp/*

# Install mbstring extension
RUN apk add --no-cache \
    oniguruma-dev \
    && docker-php-ext-install mbstring \
    && docker-php-ext-enable mbstring \
    && rm -rf /tmp/*

# INstall opcache, xdebug, redis, mongodb
RUN docker-php-source extract \
    && pecl install opcache xdebug redis mongodb apcu \
    && echo "xdebug.remote_enable=on\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_autostart=on\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_port=9000\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_handler=dbgp\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && echo "xdebug.remote_connect_back=1\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
    && docker-php-ext-enable opcache xdebug redis mongodb apcu \
    && docker-php-source delete \
    && rm -rf /tmp/*

RUN docker-php-ext-enable exif
```

### php/www.conf

```sh
; Start a new pool named 'www'.
; the variable $pool can be used in any directive and will be replaced by the
; pool name ('www' here)
[www]

; Per pool prefix
; It only applies on the following directives:
; - 'access.log'
; - 'slowlog'
; - 'listen' (unixsocket)
; - 'chroot'
; - 'chdir'
; - 'php_values'
; - 'php_admin_values'
; When not set, the global prefix (or NONE) applies instead.
; Note: This directive can also be relative to the global prefix.
; Default Value: none
;prefix = /path/to/pools/$pool

; Unix user/group of processes
; Note: The user is mandatory. If the group is not set, the default user's group
;       will be used.
user = laravel
group = laravel

; The address on which to accept FastCGI requests.
; Valid syntaxes are:
;   'ip.add.re.ss:port'    - to listen on a TCP socket to a specific IPv4 address on
;                            a specific port;
;   '[ip:6:addr:ess]:port' - to listen on a TCP socket to a specific IPv6 address on
;                            a specific port;
;   'port'                 - to listen on a TCP socket to all addresses
;                            (IPv6 and IPv4-mapped) on a specific port;
;   '/path/to/unix/socket' - to listen on a unix socket.
; Note: This value is mandatory.
listen = 127.0.0.1:9000

; Set listen(2) backlog.
; Default Value: 511 (-1 on FreeBSD and OpenBSD)
;listen.backlog = 511

; Set permissions for unix socket, if one is used. In Linux, read/write
; permissions must be set in order to allow connections from a web server. Many
; BSD-derived systems allow connections regardless of permissions. The owner
; and group can be specified either by name or by their numeric IDs.
; Default Values: user and group are set as the running user
;                 mode is set to 0660
;listen.owner = www-data
;listen.group = www-data
;listen.mode = 0660
; When POSIX Access Control Lists are supported you can set them using
; these options, value is a comma separated list of user/group names.
; When set, listen.owner and listen.group are ignored
;listen.acl_users =
;listen.acl_groups =

; List of addresses (IPv4/IPv6) of FastCGI clients which are allowed to connect.
; Equivalent to the FCGI_WEB_SERVER_ADDRS environment variable in the original
; PHP FCGI (5.2.2+). Makes sense only with a tcp listening socket. Each address
; must be separated by a comma. If this value is left blank, connections will be
; accepted from any ip address.
; Default Value: any
;listen.allowed_clients = 127.0.0.1

; Specify the nice(2) priority to apply to the pool processes (only if set)
; The value can vary from -19 (highest priority) to 20 (lower priority)
; Note: - It will only work if the FPM master process is launched as root
;       - The pool processes will inherit the master process priority
;         unless it specified otherwise
; Default Value: no set
; process.priority = -19

; Set the process dumpable flag (PR_SET_DUMPABLE prctl) even if the process user
; or group is differrent than the master process user. It allows to create process
; core dump and ptrace the process for the pool user.
; Default Value: no
; process.dumpable = yes

; Choose how the process manager will control the number of child processes.
; Possible Values:
;   static  - a fixed number (pm.max_children) of child processes;
;   dynamic - the number of child processes are set dynamically based on the
;             following directives. With this process management, there will be
;             always at least 1 children.
;             pm.max_children      - the maximum number of children that can
;                                    be alive at the same time.
;             pm.start_servers     - the number of children created on startup.
;             pm.min_spare_servers - the minimum number of children in 'idle'
;                                    state (waiting to process). If the number
;                                    of 'idle' processes is less than this
;                                    number then some children will be created.
;             pm.max_spare_servers - the maximum number of children in 'idle'
;                                    state (waiting to process). If the number
;                                    of 'idle' processes is greater than this
;                                    number then some children will be killed.
;  ondemand - no children are created at startup. Children will be forked when
;             new requests will connect. The following parameter are used:
;             pm.max_children           - the maximum number of children that
;                                         can be alive at the same time.
;             pm.process_idle_timeout   - The number of seconds after which
;                                         an idle process will be killed.
; Note: This value is mandatory.
pm = dynamic

; The number of child processes to be created when pm is set to 'static' and the
; maximum number of child processes when pm is set to 'dynamic' or 'ondemand'.
; This value sets the limit on the number of simultaneous requests that will be
; served. Equivalent to the ApacheMaxClients directive with mpm_prefork.
; Equivalent to the PHP_FCGI_CHILDREN environment variable in the original PHP
; CGI. The below defaults are based on a server without much resources. Don't
; forget to tweak pm.* to fit your needs.
; Note: Used when pm is set to 'static', 'dynamic' or 'ondemand'
; Note: This value is mandatory.
pm.max_children = 5

; The number of child processes created on startup.
; Note: Used only when pm is set to 'dynamic'
; Default Value: (min_spare_servers + max_spare_servers) / 2
pm.start_servers = 2

; The desired minimum number of idle server processes.
; Note: Used only when pm is set to 'dynamic'
; Note: Mandatory when pm is set to 'dynamic'
pm.min_spare_servers = 1

; The desired maximum number of idle server processes.
; Note: Used only when pm is set to 'dynamic'
; Note: Mandatory when pm is set to 'dynamic'
pm.max_spare_servers = 3

; The number of seconds after which an idle process will be killed.
; Note: Used only when pm is set to 'ondemand'
; Default Value: 10s
;pm.process_idle_timeout = 10s;

; The number of requests each child process should execute before respawning.
; This can be useful to work around memory leaks in 3rd party libraries. For
; endless request processing specify '0'. Equivalent to PHP_FCGI_MAX_REQUESTS.
; Default Value: 0
;pm.max_requests = 500

; The URI to view the FPM status page. If this value is not set, no URI will be
; recognized as a status page. It shows the following informations:
;   pool                 - the name of the pool;
;   process manager      - static, dynamic or ondemand;
;   start time           - the date and time FPM has started;
;   start since          - number of seconds since FPM has started;
;   accepted conn        - the number of request accepted by the pool;
;   listen queue         - the number of request in the queue of pending
;                          connections (see backlog in listen(2));
;   max listen queue     - the maximum number of requests in the queue
;                          of pending connections since FPM has started;
;   listen queue len     - the size of the socket queue of pending connections;
;   idle processes       - the number of idle processes;
;   active processes     - the number of active processes;
;   total processes      - the number of idle + active processes;
;   max active processes - the maximum number of active processes since FPM
;                          has started;
;   max children reached - number of times, the process limit has been reached,
;                          when pm tries to start more children (works only for
;                          pm 'dynamic' and 'ondemand');
; Value are updated in real time.
; Example output:
;   pool:                 www
;   process manager:      static
;   start time:           01/Jul/2011:17:53:49 +0200
;   start since:          62636
;   accepted conn:        190460
;   listen queue:         0
;   max listen queue:     1
;   listen queue len:     42
;   idle processes:       4
;   active processes:     11
;   total processes:      15
;   max active processes: 12
;   max children reached: 0
;
; By default the status page output is formatted as text/plain. Passing either
; 'html', 'xml' or 'json' in the query string will return the corresponding
; output syntax. Example:
;   http://www.foo.bar/status
;   http://www.foo.bar/status?json
;   http://www.foo.bar/status?html
;   http://www.foo.bar/status?xml
;
; By default the status page only outputs short status. Passing 'full' in the
; query string will also return status for each pool process.
; Example:
;   http://www.foo.bar/status?full
;   http://www.foo.bar/status?json&full
;   http://www.foo.bar/status?html&full
;   http://www.foo.bar/status?xml&full
; The Full status returns for each process:
;   pid                  - the PID of the process;
;   state                - the state of the process (Idle, Running, ...);
;   start time           - the date and time the process has started;
;   start since          - the number of seconds since the process has started;
;   requests             - the number of requests the process has served;
;   request duration     - the duration in µs of the requests;
;   request method       - the request method (GET, POST, ...);
;   request URI          - the request URI with the query string;
;   content length       - the content length of the request (only with POST);
;   user                 - the user (PHP_AUTH_USER) (or '-' if not set);
;   script               - the main script called (or '-' if not set);
;   last request cpu     - the %cpu the last request consumed
;                          it's always 0 if the process is not in Idle state
;                          because CPU calculation is done when the request
;                          processing has terminated;
;   last request memory  - the max amount of memory the last request consumed
;                          it's always 0 if the process is not in Idle state
;                          because memory calculation is done when the request
;                          processing has terminated;
; If the process is in Idle state, then informations are related to the
; last request the process has served. Otherwise informations are related to
; the current request being served.
; Example output:
;   ************************
;   pid:                  31330
;   state:                Running
;   start time:           01/Jul/2011:17:53:49 +0200
;   start since:          63087
;   requests:             12808
;   request duration:     1250261
;   request method:       GET
;   request URI:          /test_mem.php?N=10000
;   content length:       0
;   user:                 -
;   script:               /home/fat/web/docs/php/test_mem.php
;   last request cpu:     0.00
;   last request memory:  0
;
; Note: There is a real-time FPM status monitoring sample web page available
;       It's available in: /usr/local/share/php/fpm/status.html
;
; Note: The value must start with a leading slash (/). The value can be
;       anything, but it may not be a good idea to use the .php extension or it
;       may conflict with a real PHP file.
; Default Value: not set
;pm.status_path = /status

; The ping URI to call the monitoring page of FPM. If this value is not set, no
; URI will be recognized as a ping page. This could be used to test from outside
; that FPM is alive and responding, or to
; - create a graph of FPM availability (rrd or such);
; - remove a server from a group if it is not responding (load balancing);
; - trigger alerts for the operating team (24/7).
; Note: The value must start with a leading slash (/). The value can be
;       anything, but it may not be a good idea to use the .php extension or it
;       may conflict with a real PHP file.
; Default Value: not set
;ping.path = /ping

; This directive may be used to customize the response of a ping request. The
; response is formatted as text/plain with a 200 response code.
; Default Value: pong
;ping.response = pong

; The access log file
; Default: not set
;access.log = log/$pool.access.log
```

### nginx/default.conf

```
server {
    listen 80;
    index index.php index.html;
    server_name localhost;
    root /var/www/html/laravel/public;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass php:9000;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param PATH_INFO $fastcgi_path_info;
    }
}
```

### nginx/nginx.conf

```
user laravel;
worker_processes  auto;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}
```

# README

## docker-compose-laravel
A pretty simplified Docker Compose workflow that sets up a LEMP network of containers for local Laravel development. You can view the full article that inspired this repo [here](https://dev.to/aschmelyun/the-beauty-of-docker-for-local-laravel-development-13c0).


## Usage

To get started, make sure you have [Docker installed](https://docs.docker.com/docker-for-mac/install/) on your system, and then clone this repository.

Next, navigate in your terminal to the directory you cloned this, and spin up the containers for the web server by running `docker-compose up -d --build site`.

After that completes, follow the steps from the [src/README.md](src/README.md) file to get your Laravel project added in (or create a new blank one).

Bringing up the Docker Compose network with `site` instead of just using `up`, ensures that only our site's containers are brought up at the start, instead of all of the command containers as well. The following are built for our web server, with their exposed ports detailed:

- **nginx** - `:8080`
- **mysql** - `:3306`
- **php** - `:9000`

Three additional containers are included that handle Composer, NPM, and Artisan commands *without* having to have these platforms installed on your local computer. Use the following command examples from your project root, modifying them to fit your particular use case.

- `docker-compose run --rm composer update`
- `docker-compose run --rm npm run dev`
- `docker-compose run --rm artisan migrate` 

## Persistent MySQL Storage

By default, whenever you bring down the Docker network, your MySQL data will be removed after the containers are destroyed. If you would like to have persistent data that remains after bringing containers down and back up, do the following:

1. Create a `mysql` folder in the project root, alongside the `nginx` and `src` folders.
2. Under the mysql service in your `docker-compose.yml` file, add the following lines:

```
volumes:
  - ./mysql:/var/lib/mysql
```

## Database Docker Config

path: databases/storage

```yaml
version: '3.9'

networks:
    shared:
        # driver: bridge
        # driver_opts:
        #     com.docker.network.enable_ipv6: "false"
        # ipam:
        #     driver: bridge
        #     config:
        #         - subnet: 172.20.0.0/16
        #         gateway: 172.20.0.1
        external:
            name: local-bridge-net

services:
    database:
        image: mysql
        container_name: db-mysql
        restart: unless-stopped
        tty: true
        ports:
            - 3306:3306
        environment:
            MYSQL_DATABASE: homestead
            MYSQL_USER: homestead
            MYSQL_PASSWORD: secret
            MYSQL_ROOT_PASSWORD: secret
            SERVICE_TAGS: dev
            SERVICE_NAME: mysql
        networks:
            shared:
                # ipv4_address: 172.20.0.10
        expose:
          # Opens port 3306 on the container
          - 3306
          # Where our data will be persisted
        volumes:
          - storage_mysql:/var/lib/mysql
    redis:
        image: redis
        container_name: cache-redis
        ports:
            - 6379:6379
        expose:
          # Opens port 3306 on the container
          - 6379
        volumes:
            - storage_redis:/data
        networks:
            shared:
                # ipv4_address: 172.20.0.11
    mongo:
        image: mongo
        container_name: cache-mongo
        ports:
            - 27017:27017
        expose:
          # Opens port 3306 on the container
          - 27017
        volumes:
            - storage_mongo:/data
        networks:
            shared:
                # ipv4_address: 172.20.0.12
# Names our volume
volumes:
    storage_mysql:
    storage_redis:
    storage_mongo:
```

## Golang Docker Config

path: golang/{data/{mongo,redis},go/{bin,src,pkg}}

```yaml
version: '3.9'

networks:
    shared:
        external:
            name: local-bridge-net
        
services:
    app:
        image: golang:latest
        container_name: golang_app
        working_dir: /go/src/
        build:
            context: ./go
        volumes:
            - ./go/:/go/
        ports:
            - "8000:8080"
        environment:
            WORKING_DIR: /go/src/
        # command: go run /go/src/example.com/http_demo/main.go
        tty: true 
        networks:
            - shared
```
  
Dockerfile
  
```
FROM golang:latest

RUN apt-get update
RUN apt-get install vim -y
RUN go get "github.com/go-sql-driver/mysql"
```

## Install PHP Redis Extension

```sh
$ apk add --update --no-cache autoconf g++ make
$ pecl install redis
$ docker-php-ext-enable redis
```

## Sample Laravel .env

```
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:wi5SlI3fHzFtKXwy607/YfarZUi4cJ+inxp7V7unMZA=
APP_DEBUG=true
APP_URL=http://localhost:8088

LOG_CHANNEL=stack
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=172.20.0.8
DB_PORT=3306
DB_DATABASE=db-cronjob
DB_USERNAME=root
DB_PASSWORD=secret

BROADCAST_DRIVER=log
CACHE_DRIVER=redis
# default QUEUE_CONNECTION=sync
QUEUE_CONNECTION=database
SESSION_DRIVER=file
SESSION_LIFETIME=120

# MEMCACHED_HOST=127.0.0.1

REDIS_HOST=172.20.0.4
REDIS_PASSWORD=null
REDIS_PORT=6379
REDIS_READ_TIMEOUT=60

MAIL_MAILER=smtp
# default MAIL_HOST=mailhog
MAIL_HOST=smtp.mailtrap.io
# default MAIL_PORT=1025
MAIL_PORT=2525
MAIL_USERNAME=e46ecee7a6c3eb
MAIL_PASSWORD=1b93833a0a51fd
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=bab0413d38-fe11f5@inbox.mailtrap.io
MAIL_FROM_NAME="${APP_NAME}"

# AWS S3 CONFIGURATIONS
AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=

PUSHER_APP_ID=
PUSHER_APP_KEY=
PUSHER_APP_SECRET=
PUSHER_APP_CLUSTER=mt1

MIX_PUSHER_APP_KEY="${PUSHER_APP_KEY}"
MIX_PUSHER_APP_CLUSTER="${PUSHER_APP_CLUSTER}"

# POST CONFIGURATIONS
POSTS_PER_PAGE=1

JWT_SECRET=X4ZKhcSUoDKp2ViaIGJS4yjZUUWTFhDxCGM1pTPcXg7XHokIT8YF8G7D9VsyBfiI
```
