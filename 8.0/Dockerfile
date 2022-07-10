FROM php:8.0-apache

LABEL maintainer="Angel Aviel Domaoan <dev.tenshiamd@gmail.com>"

LABEL org.opencontainers.image.source "https://github.com/tenshiAMD/docker-php"

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"

## Installs dependencies
RUN apt-get update && apt-get install -y \
	libmcrypt-dev libreadline-dev \ 
	libxml2-dev libxml++2.6-dev \
	libpng-dev libssl-dev libcurl4-openssl-dev \
    libzip-dev libonig-dev pkg-config  \
    && docker-php-ext-install -j$(nproc) mysqli \
    && docker-php-ext-install -j$(nproc) mbstring \
    && docker-php-ext-install -j$(nproc) xml \
    && docker-php-ext-install -j$(nproc) pdo pdo_mysql \
	&& docker-php-ext-install -j$(nproc) zip \
    && docker-php-ext-install -j$(nproc) curl \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-install -j$(nproc) exif

## Installs utilities
RUN apt-get update && apt-get install -y curl vim nano iputils-ping

## Installs Composer
RUN curl -sS https://getcomposer.org/installer | php
RUN chmod +x composer.phar
RUN mv composer.phar /usr/bin
RUN ln -s /usr/bin/composer.phar /usr/bin/composer

## Installs MySQL client for CLI
RUN apt-get update && apt-get install -y default-mysql-client

## Installs Node
RUN apt-get update && apt-get install -y gnupg2 
RUN curl -sL https://deb.nodesource.com/setup_16.x | bash -
RUN apt-get update && apt-get install -y nodejs 

## Installs `dockerize`
RUN apt-get update && apt-get install -y wget
ENV DOCKERIZE_VERSION v0.6.1
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

## Run system cleanup
RUN apt-get clean
RUN apt-get autoremove -y --purge

## Enables rewrite module for apache2
RUN a2enmod rewrite

EXPOSE 80
