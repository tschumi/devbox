# devbox

## Description

This is my personal little php development stack containig the following:

* Apache 2.4.46
* PHP 8.0-fpm
* MariaDB 10.5.8

It will serving you at the following ports:

* 80 (Website) - Example: https://localhost
* 8080 (PhpMyAdmin) - Example: http://localhost:8080
* 3306 (MySql DB)
* 9003 (Xdebug)

> This is for development purpose only - please do never ever run this in production!

This is inspired by so many of you out there, as i looked at many repos and tutorials. Unfortunatly i don't remember all the names and websites - so thanks to everyone who is posting tutorials and to everyone who is sharing her/his work openly.

## Prequisites

You need docker and docker-compose installed in your environment. If you're new to the docker world, don't worry. Visit:
https://www.docker.com/get-started

## General information

The `public_html` folder is mounted into the `/var/www/html/` folder in the container.

The database will be stored in a volume called `db-data` and persists.

For SSL support, please place your own `cert.pem` and `key.pem` to the `httpd/certs` folder.

The following php extensions are installed:

* GD
* MySqli
* Enchant
* Xdebug

## Installation

1. First clone the repo

```
git clone https://github.com/tschumi/devbox devbox
```

2. Change to the created folder

```
cd devbox
```

3. Then create the SSL certs if you diden't allready

To create the SSL certs i recommend [mkcert](https://github.com/FiloSottile/mkcert). It will create a locally-trustet development certificate. Just follow the installation instructions for your environment. The first command requires elevated rights, as it adds the created local CA (Certificate Authority) to the trust store of your environment. Change yourdomain.com to your machine name or delete it if you want to stick with localhost.
```
mkcert -install
mkcert -cert-file httpd/certs/cert.pem -key-file httpd/certs/key.pem yourdomain.com devbox localhost 127.0.0.1 ::1
```

4. Change the root password of the database

Open docker-compose.yml and edit the following line
```
MYSQL_ROOT_PASSWORD=yourownsupersecretpassword
```
If you want to keep the fast db connection check on index.php alive, don't forget to update the password in `public_html/index.php` as well.

5. Build and run it

```
docker-compose up
```

## Different PHP versions

Changing the PHP version is easy as 1, 2, 3. Just change the `PHP_TAG=` in the docker-compose.yml as desired. The following versions have been tested:

* 8.0-fpm-alpine3.13
* 7.4-fpm-alpine3.11
* 7.3-fpm-alpine3.11
* 7.2-fpm-alpine3.11
* 7.1-fpm-alpine3.10
* 5.6-fpm-alpine3.8

If you're asking yourself, why i've choosen the 3.11 versions of 7.2, 7.3 and 7.4, it's because the newer versions do not support enchant, as described here:
https://github.com/mlocati/docker-php-extension-installer#special-requirements

## Debugging

Xdebug is installed and configured to listen on default port 9003. The debugger has to be triggered. The recommended way to do it is with one of the following browser extensions: https://xdebug.org/docs/step_debug#browser-extensions

You have to the setup your prefered code editor for debugging. If you use VS Code or VS Codium, which i can highly recommend, a configuration file is shipped with this repo. All you need to do is to switch to run view in the Activity Bar and run the preconfigured `XDebug@devbox`.
