ARG PHP_TAG=8.0-fpm-alpine3.13

FROM php:${PHP_TAG}

RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini
COPY custom.ini /usr/local/etc/php/conf.d/

RUN apk update; \
    apk upgrade;

# Easy installation of PHP extensions in official PHP Docker images
# https://github.com/mlocati/docker-php-extension-installer
ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/

RUN chmod +x /usr/local/bin/install-php-extensions && sync && \
    install-php-extensions enchant gd mysqli xdebug

# Write config file for xdebug depending on Xdebug version
# Hint for string contains comparison came from: https://stackoverflow.com/a/39164226
RUN if php -v | grep "Xdebug v3" - > /dev/null ; \
    then \
        touch /usr/local/etc/php/conf.d/xdebug3.ini \
        && echo xdebug.mode=develop,debug,coverage >> /usr/local/etc/php/conf.d/xdebug3.ini \
        && echo xdebug.client_host=host.docker.internal >> /usr/local/etc/php/conf.d/xdebug3.ini; \
    else \
        touch /usr/local/etc/php/conf.d/xdebug2.ini \
        && echo xdebug.remote_enable=1 >> /usr/local/etc/php/conf.d/xdebug2.ini \
        && echo xdebug.remote_host=host.docker.internal >> /usr/local/etc/php/conf.d/xdebug2.ini \
        && echo xdebug.remote_port=9003 >> /usr/local/etc/php/conf.d/xdebug2.ini; \
    fi;