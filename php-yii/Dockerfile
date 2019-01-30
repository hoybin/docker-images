ARG PHP_BASE_IMAGE_VERSION
FROM php:${PHP_BASE_IMAGE_VERSION}-fpm-alpine

LABEL maintainer="hoybin@outlook.com"

COPY image-files/ /

# Install system packages for PHP extensions recommended for Yii 2.0 Framework
RUN apk --update --virtual build-deps add \
            autoconf \
            dpkg-dev \
            icu-dev \
            freetype-dev \
            imagemagick-dev \
            libjpeg-turbo-dev \
            libpng-dev \
            libxml2-dev \
            libmemcached-dev \
            postgresql-dev \
            pcre-dev \
            libtool \
            make \
            gcc \
            g++ \
            file \
            re2c \
            dpkg \
    && apk add \
            git \
            bash \
            bash-completion \
            icu \
            freetype \
            imagemagick \
            libintl \
            libjpeg-turbo \
            libpng \
            libltdl \
            libxml2 \
            libmemcached \
            mysql-client \
            postgresql \
            pcre \
    && docker-php-ext-configure gd \
            --with-freetype-dir=/usr/include/ \
            --with-png-dir=/usr/include/ \
            --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install \
            gd \
            bcmath \
            soap \
            zip \
            exif \
            intl \
            opcache \
            pdo_mysql \
            pdo_pgsql

RUN pecl install imagick
RUN pecl install apcu
RUN pecl install memcached
RUN pecl install redis
RUN pecl install mongodb
RUN pecl install xdebug

RUN docker-php-ext-enable imagick apcu memcached redis mongodb \
    && apk del build-deps

ENV PATH=/var/www/html:/var/www/html/vendor/bin:/root/.composer/vendor/bin:$PATH \
    COMPOSER_ALLOW_SUPERUSER=1 \
    COMPOSER_REPO_URL="" \
    GITHUB_API_TOKEN="" \
    PHP_USER_ID=1000 \
    PHP_ENABLE_XDEBUG=0 \
    PHP_DEVELOP_ENV=0

# Add GITHUB_API_TOKEN support for composer
RUN chmod 700 \
        /usr/local/bin/docker-php-entrypoint \
        /usr/local/bin/composer

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- \
        --filename=composer.phar --install-dir=/usr/local/bin

# Install composer plugins
RUN composer global require --optimize-autoloader "hirak/prestissimo" && \
    composer global dumpautoload --optimize && \
    composer clear-cache

VOLUME ["/var/www/html"]
