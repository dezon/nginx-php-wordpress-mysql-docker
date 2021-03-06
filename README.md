nginx-php-wordpress-mysql-docker
================================

## About

Setup a local development environment for Wordpress using Nginx, PHP-FPM, Wordpress and MYSQL docker containers. 

This repository uses docker-compose to link everything together.

The only required configuration is to set the environment variable `WORDPRESS_DIR` which will tell the php-fpm container where the core wordpress files are located on your local machine. 

eg. 

`export WORDPRESS_DIR=/Users/jharrington/wordpress`


## Wordpress

You will need to change the following in the wp-config.php:

```
define('DB_NAME', 'wordpress');
define('DB_USER', 'root');
define('DB_PASSWORD', '');
define('DB_HOST', "db:3306");
define('DB_CHARSET', 'utf8');
define('DB_COLLATE', '');

define('AUTH_KEY',         'put your unique phrase here');
define('SECURE_AUTH_KEY',  'put your unique phrase here');
define('LOGGED_IN_KEY',    'put your unique phrase here');
define('NONCE_KEY',        'put your unique phrase here');
define('AUTH_SALT',        'put your unique phrase here');
define('SECURE_AUTH_SALT', 'put your unique phrase here');
define('LOGGED_IN_SALT',   'put your unique phrase here');
define('NONCE_SALT',       'put your unique phrase here');

```

## Mac OSX

If you don't have docker installed follow the Docker Toolbox instructions here: http://docs.docker.com/engine/installation/mac/

Run the following to create a new Docker VM and setup the hostfile entry which you'll reference to access your Wordpress site.

`docker-machine create --driver=virtualbox --virtualbox-memory=1024 --virtualbox-cpu-count=4 docker-vm`

`eval "$(docker-machine env docker-vm)"`

`echo "$(docker-machine ip docker-vm) dockerhost" | sudo tee -a /etc/hosts`

## Setup

Clone repo:

`git@github.com:jharrington22/nginx-php-wordpress-mysql-docker.git`

`cd nginx-php-wordpress-mysql-docker`

Set ENV for Wordpress core files location:

`export WORDPRESS_DIR=<Wordpress directory>`

Build images:

`docker-compose build`

Bring up environment:

`docker-compose up`

Next browse to http://localhost or if you are using a Mac or Windows http://dockerhost

## MYSQL

The MySQL instance has the following credentials, no password:

Username: root
Password: 
Host: localhost or dockerhost (Mac, Windows)

You can restore/dump your database with:

### restore

`mysql -u root -p -h dockerhost wordpress < mysql-dump-file.sql`

### dump

`mysqldump -u root -p -h dockerhost wordpress > mysql-dump-file.sql`

Remember to replace dockerhost with localhost if you are on a Linux host.

## PHP

If you need extra PHP packages you can install them by editing the Dockerfile under php-fpm. 

Eg. To add php-json change the RUN apk add line to 

`RUN apk add --update php-fpm php-mysql php php-json`

See available packages here:

https://pkgs.alpinelinux.org/packages?name=php%25&repo=all&arch=x86_64&maintainer=all
