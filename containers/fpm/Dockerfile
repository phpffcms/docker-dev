FROM php:7.2-fpm-alpine
# prepare apk manager
RUN apk add --no-cache bash sed
RUN set -ex
# install compilers, custom libraries for php extending
RUN apk add --no-cache coreutils \
    freetype \
    libpng \
    libjpeg \
    libjpeg-turbo \
    libjpeg-turbo-dev \ 
    freetype-dev \
    libpng-dev \
    libvpx-dev \
    libwebp-dev \
    curl \
    curl-dev \
    libcurl \
    git \
    libressl-dev \
    ca-certificates \
    linux-headers \
    libmcrypt-dev
# configure & compile additional GD lib, todo: /usr -> /usr/include
RUN docker-php-ext-configure gd \
    --with-gd \
    --with-freetype-dir=/usr/include \
    --with-png-dir=/usr/include \
    --with-vpx-dir=/usr/lib \
    --with-webp-dir=/usr/lib \
    --with-jpeg-dir=/usr/include && \
 NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
 docker-php-ext-install -j${NPROC} gd
RUN docker-php-ext-install iconv exif opcache pdo pdo_mysql mbstring curl
# enable ext
RUN docker-php-ext-enable iconv curl gd opcache
# prepare opcache configurations
COPY opcache.ini /usr/local/etc/php/conf.d/opcache.ini
# install composer 
RUN EXPECTED_COMPOSER_SIGNATURE=$(wget -q -O - https://composer.github.io/installer.sig) && \
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
php -r "if (hash_file('SHA384', 'composer-setup.php') === '${EXPECTED_COMPOSER_SIGNATURE}') { echo 'Composer.phar Installer verified'; } else { echo 'Composer.phar Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
php composer-setup.php --install-dir=/usr/bin --filename=composer && \
php -r "unlink('composer-setup.php');"  && \
# remove useless packages after compile
apk del linux-headers 
# run composer and create project
# RUN composer create-project phpffcms/ffcms /www --keep-vcs --prefer-dist \
# && composer update \
# && php -f /www/console.php main:install --db=database --user ...
# mark use volume
VOLUME ["/www"]
# define entry-point command
CMD ["php-fpm"]