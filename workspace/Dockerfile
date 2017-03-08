FROM php:7.0-alpine

MAINTAINER Gabriel Vieira <gabrielvie@gmail.com>

#------------------------
# Packages
#------------------------
RUN apk --update --no-cache add \
    autoconf gcc g++ make \
    postgresql-dev libxml2-dev \
    bash git tzdata \
    mysql-client

#------------------------
# Set Container TimeZone
#------------------------
ARG TIMEZONE=UTC
RUN cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
 && echo "${TIMEZONE}" > /etc/timezone \
 && apk del tzdata

#------------------------
# Add User
#------------------------
ARG PUID=1000
ARG PGID=1000
RUN addgroup -g ${PGID} dev-user \
 && adduser -u ${PUID} -D -S -G dev-user dev-user

#------------------------
# PHP ini
#------------------------
ARG MEMORY_LIMIT=128M
RUN touch /usr/local/etc/php/php.ini \
 && echo "memory_limit = ${MEMORY_LIMIT}" >> /usr/local/etc/php/php.ini \
 && echo "date.timezone = ${TIMEZONE}" >> /usr/local/etc/php/php.ini

#------------------------
# Composer
#------------------------
RUN curl -s http://getcomposer.org/installer | php \
 && echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc \
 && mv composer.phar /usr/local/bin/composer

#------------------------
# PHP Extensions
#------------------------
RUN docker-php-ext-install \
    pdo pdo_mysql pdo_pgsql \
    ctype dom

#------------------------
# Xdebug Extensions
#------------------------
RUN yes | pecl install xdebug \
 && docker-php-ext-enable xdebug
ADD ./xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

#------------------------
# Bash config
#------------------------
USER dev-user
RUN git clone --depth=1 https://github.com/Bash-it/bash-it.git /home/dev-user/.bash_it \
 && /home/dev-user/.bash_it/install.sh

#------------------------
# Composer configuration
#------------------------
RUN mkdir /home/dev-user/.composer
ADD ./composer.json /home/dev-user/.composer/composer.json
RUN composer global install \
 && echo 'export PATH="$PATH:/var/www/vendor/bin:$HOME/.composer/vendor/bin"' >> ~/.bashrc

USER root
RUN rm -rf /tmp/*

USER dev-user

WORKDIR /var/www

ENTRYPOINT ["/bin/bash"]
