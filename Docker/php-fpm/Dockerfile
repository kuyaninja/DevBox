FROM php:7.2-fpm-alpine3.12

LABEL maintainer="hallo@kuya.ninja"

RUN apk update

# bash
RUN apk add --update --no-cache bash

# dependencies
ENV PHPIZE_DEPS \
    build-base \
    autoconf \
    libtool \
    pcre2-dev \
    cmake \
    file \
    pkgconf \
    re2c \
    git \
    wget \
    pcre-dev

RUN apk add --update --no-cache $PHPIZE_DEPS

# intl, zip, soap
RUN apk add --update --no-cache libintl icu icu-dev libxml2-dev \
    && docker-php-ext-install intl zip soap

# mysqli, pdo, pdo_mysql, pdo_pgsql
RUN apk add --update --no-cache postgresql-dev \
    && docker-php-ext-install mysqli pdo pdo_mysql pdo_pgsql

# gd, iconv, libltdl
RUN apk add --update --no-cache \
        freetype-dev \
        libjpeg-turbo-dev \
        libpng-dev \
        libltdl \
    && docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" iconv \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j"$(getconf _NPROCESSORS_ONLN)" gd

# gmp
RUN apk add --update --no-cache gmp gmp-dev \
    && docker-php-ext-install gmp

# php-redis
ENV PHPREDIS_VERSION="5.3.2"

RUN docker-php-source extract \
    && curl -L -o /tmp/redis.tar.gz "https://github.com/phpredis/phpredis/archive/${PHPREDIS_VERSION}.tar.gz" \
    && tar xfz /tmp/redis.tar.gz \
    && rm -r /tmp/redis.tar.gz \
    && mv phpredis-$PHPREDIS_VERSION /usr/src/php/ext/redis \
    && docker-php-ext-install redis \
    && docker-php-source delete

# imagick
RUN apk add --update --no-cache imagemagick-dev \
    && pecl install imagick \
    && docker-php-ext-enable imagick

# ssh2
RUN apk add --update --no-cache libssh2 libssh2-dev \
    && pecl install ssh2-1 \
    && docker-php-ext-enable ssh2

# opcache
RUN docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache

# bcmath, mbstring
RUN docker-php-ext-install bcmath mbstring

# xdebug
RUN pecl install xdebug-2.9.8 \
    && docker-php-ext-enable xdebug

# oauth
RUN pecl install oauth-2.0.7 \
    && docker-php-ext-enable oauth

# openldap
RUN apk add --update --no-cache openldap-dev \
    && docker-php-ext-configure ldap --with-libdir=lib/ \
    && docker-php-ext-install ldap

# phalcon
ARG PSR_VERSION=1.0.0
ARG PHALCON_VERSION=4.1.0
ARG PHALCON_EXT_PATH=php7/64bits
ARG TIMEZONE=Asia/Jakarta

RUN apk update \
    && apk add ca-certificates \
    && update-ca-certificates

RUN set -xe && \
        # Download PSR, see https://github.com/jbboehr/php-psr
        curl -LO https://github.com/jbboehr/php-psr/archive/v${PSR_VERSION}.tar.gz && \
        tar xzf ${PWD}/v${PSR_VERSION}.tar.gz && \
        # Download Phalcon
        curl -LO https://github.com/phalcon/cphalcon/archive/v${PHALCON_VERSION}.tar.gz && \
        tar xzf ${PWD}/v${PHALCON_VERSION}.tar.gz && \
        docker-php-ext-install -j $(getconf _NPROCESSORS_ONLN) \
            ${PWD}/php-psr-${PSR_VERSION} \
            ${PWD}/cphalcon-${PHALCON_VERSION}/build/${PHALCON_EXT_PATH} \
        && \
        # Remove all temp files
        rm -r \
            ${PWD}/v${PSR_VERSION}.tar.gz \
            ${PWD}/php-psr-${PSR_VERSION} \
            ${PWD}/v${PHALCON_VERSION}.tar.gz \
            ${PWD}/cphalcon-${PHALCON_VERSION} \
        && \
        php -m

COPY ./Docker/php-fpm/docker-phalcon-* /usr/local/bin/
RUN chmod +x /usr/local/bin/docker-phalcon-install-devtools \
    && /usr/local/bin/docker-phalcon-install-devtools

# log file
RUN touch /var/log/php-fpm.log \
    && chmod 777 /var/log/php-fpm.log

# app folder
WORKDIR /var/www/app

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

# install Laravel
#RUN composer global require laravel/installer
ENV PATH "$PATH:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/root/.composer/vendor/bin"

# port
EXPOSE 9000

# remove dependencies
RUN apk del $PHPIZE_DEPS

# clear cache
RUN rm -rf /tmp/* /var/cache/apk/*

# start
CMD ["php-fpm"]