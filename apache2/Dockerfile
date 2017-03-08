FROM php:7.0-apache

RUN apt-get update \
 && apt-get install -y \
    autoconf gcc g++ make \
    libpq-dev libxml2-dev \
    libfreetype6 libfreetype6-dev \
    libjpeg-dev libpng-dev

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
 && docker-php-ext-install gd

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

RUN rm /etc/apache2/sites-enabled/000-default.conf

WORKDIR /var/www

EXPOSE "80" "443"
