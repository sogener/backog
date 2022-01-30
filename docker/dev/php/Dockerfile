FROM composer:latest as composer
FROM php:8.1-fpm-alpine as php

RUN set -xe && apk update && apk upgrade

RUN set -xe \
    && apk add --no-cache \
    mysql-dev

RUN docker-php-ext-install -j$(nproc) \
    pdo_mysql \
    pdo

#RUN apk add gnu-libiconv --no-cache --repository http://dl-cdn.alpinelinux.org/alpine/edge/community/ --allow-untrusted

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN composer -V

WORKDIR /var/www/app
ENV COMPOSER_ALLOW_SUPERUSER=1
ENV COMPOSER_MEMORY_LIMIT=-1

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}


RUN echo http://dl-2.alpinelinux.org/alpine/edge/community/ >> /etc/apk/repositories

RUN apk --no-cache add shadow && usermod -u 1000 www-data && groupmod -g 1000 www-data
#RUN usermod -u ${PUID} www-data && groupmod -g ${PGID} www-data

RUN mkdir -p /var/www/app && chown -R www-data:www-data /var/www/app

RUN echo 'memory_limit=1536M' > /usr/local/etc/php/conf.d/memory.ini
RUN echo 'upload_max_filesize = 10M' > /usr/local/etc/php/conf.d/upload-size.ini
RUN echo 'post_max_size = 10M' >> /usr/local/etc/php/conf.d/upload-size.ini

USER ${PUID}:${PGID}
