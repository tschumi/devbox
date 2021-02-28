FROM centos:7
RUN yum update -y && \
    yum install -y epel-release

RUN yum install -y autoconf make gcc php-fpm php-devel php-pear php-xml php-cli \
                   php-bcmath php-dba php-gd php-intl php-mbstring php-mysql php-pdo php-soap \
                   php-pecl-apcu php-pecl-imagick php-enchant php-pecl-xdebug

COPY custom.ini /etc/php.d/
COPY www.conf /etc/php-fpm.d/

RUN touch /etc/php.d/xdebug.ini \
        && echo [xdebug] >> /etc/php.d/xdebug.ini \
        && echo xdebug.remote_enable=1 >> /etc/php.d/xdebug.ini \
        && echo xdebug.remote_host=host.docker.internal >> /etc/php.d/xdebug.ini \
        && echo xdebug.remote_port=9003 >> /etc/php.d/xdebug.ini;

RUN yum clean all

CMD ["php-fpm"]

EXPOSE 9000
