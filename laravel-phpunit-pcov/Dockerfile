FROM ubuntu

ENV COMPOSER_ALLOW_SUPERUSER 1
ENV DEBIAN_FRONTEND noninteractive
ENV PHP_VERSION 7.2
ENV DRIVER pcov

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

RUN apt-get update && apt-get install -y --no-install-recommends apt-utils
RUN apt-get install -y --no-install-recommends \
    php${PHP_VERSION} \
    php${PHP_VERSION}-zip \
    php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-xml \
    php${PHP_VERSION}-dev \
    make \
    unzip \
    git \
    ca-certificates \
    re2c \
    ;

RUN git clone https://github.com/krakjoe/pcov /home/root/pcov
WORKDIR /home/root/pcov
RUN git checkout v1.0.0 \
    && phpize \
    && ./configure \
    && make clean install \
    && echo "extension=pcov.so" > /etc/php/${PHP_VERSION}/cli/conf.d/pcov.ini \
    ;

RUN composer create-project --prefer-dist laravel/laravel /home/root/blog
WORKDIR /home/root/blog
RUN composer remove --dev phpunit/phpunit \
    && composer require --dev phpunit/phpunit phpunit/php-code-coverage \
    ;

RUN vendor/bin/phpunit --coverage-text