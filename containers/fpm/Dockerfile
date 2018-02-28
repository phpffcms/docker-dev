FROM php:7.2-fpm-alpine

RUN apk add --no-cache bash sed
RUN set -ex

# install compiler libs
RUN apk add --no-cache --virtual .build-deps libjpeg-turbo-dev libpng-dev
# configure & compile additional libs for php
RUN docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr;
RUN docker-php-ext-install gd opcache pdo pdo_mysql mbstring

# cleanup
RUN apk del .build-deps

# prepare opcache configurations
COPY opcache.ini /usr/local/etc/php/conf.d/opcache.ini

# mark use volume
VOLUME ["/www"]

# define entry-point command
CMD ["php-fpm"]