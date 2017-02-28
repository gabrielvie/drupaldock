FROM php:7.0-fpm-alpine

MAINTAINER Gabriel Vieira <gabrielvie@gmail.com>

#------------------------
# Packages
#------------------------
RUN apk --update --no-cache add \
    autoconf gcc g++ make \
    postgresql-dev libxml2-dev \
    freetype freetype-dev \
    libjpeg-turbo libpng-dev \
    libjpeg-turbo-dev libpng

#------------------------
# PHP Extensions
#------------------------
RUN docker-php-ext-install \
    pdo pdo_mysql pdo_pgsql \
    ctype dom

RUN docker-php-ext-configure gd \
    --with-gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
 && docker-php-ext-install gd \
 && apk del --no-cache \
    freetype-dev \
    libpng-dev \
    libjpeg-turbo-dev

#------------------------
# Xdebug Extension
#------------------------
RUN yes | pecl install xdebug \
 && docker-php-ext-enable xdebug
COPY ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#------------------------
# OPCache Extension
#------------------------
RUN docker-php-ext-install opcache \
 && docker-php-ext-enable opcache
COPY ./opcache.ini /usr/local/etc/php/conf.d/opcache.ini

WORKDIR /var/www

CMD ["php-fpm"]

EXPOSE 9000
