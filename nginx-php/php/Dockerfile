FROM php:7.3.3-fpm

RUN apt-get update && apt-get install -y zip \
    unzip procps wget zlib1g-dev cron vim net-tools telnet \
    libfreetype6-dev libjpeg62-turbo-dev libpng-dev libicu-dev libzip-dev \
    && pecl install redis-4.0.1 \
    && docker-php-ext-enable redis \
    && docker-php-ext-install -j$(nproc) pdo_mysql \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install -j$(nproc) bcmath \
    && docker-php-ext-install -j$(nproc) zip \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
