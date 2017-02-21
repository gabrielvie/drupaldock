FROM php:7.0-fpm-alpine

MAINTAINER Gabriel Vieira <gabrielvie@gmail.com>

#------------------------
# Packages
#------------------------
RUN apk --update --no-cache add \
    autoconf gcc g++ make

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
