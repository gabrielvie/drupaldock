FROM php:7.0-alpine

MAINTAINER Gabriel Vieira <gabrielvie@gmail.com>

#------------------------
# Workspace bash
#------------------------
RUN apk update \
 && apk --no-cache add \
    bash git tzdata openssh

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
# Composer
#------------------------
RUN curl -s http://getcomposer.org/installer | php \
 && echo "export PATH=${PATH}:/var/www/vendor/bin" >> ~/.bashrc \
 && mv composer.phar /usr/local/bin/composer

#------------------------
# Bash config
#------------------------
USER dev-user
RUN git clone --depth=1 https://github.com/Bash-it/bash-it.git /home/dev-user/.bash_it \
 && /home/dev-user/.bash_it/install.sh

    # git mysql-client postgresql-client curl \
    # libmcrypt libmcrypt-dev libxml2-dev \
    # libpng-dev libjpeg-turbo-dev

# RUN docker-php-source extract \
#  && pecl install xdebug \
#  && docker-php-ext-enable xdebug \
#  && docker-php-source delete \
#  && docker-php-ext-install \
#     dom exif iconv json mbstring mcrypt mysqli opcache \
#     pdo pdo_mysql pdo_pgsql pdo_sqlite pgsql \
#     session simplexml tidy tokenizer xml xmlreader xmlrpc xmlwriter xsl zip \
#  && echo "xdebug.remote_enable=on\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#  && echo "xdebug.remote_autostart=off\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#  && echo "xdebug.remote_port=9000\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#  && echo "xdebug.remote_handler=dbgp\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini \
#  && echo "xdebug.remote_connect_back=0\n" >> /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini

#------------------------
# Composer configuration
#------------------------
RUN mkdir /home/dev-user/.composer
COPY ./composer.json /home/dev-user/.composer/composer.json
RUN composer global install \
 && echo 'export PATH="$PATH:/var/www/vendor/bin:$HOME/.composer/vendor/bin"' >> ~/.bashrc


USER root
RUN rm -rf /tmp/*

USER dev-user

WORKDIR /var/www

ENTRYPOINT ["/bin/bash"]
