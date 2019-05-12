FROM php:7.1-apache

ENV TZ Asia/Tokyo
RUN echo "${TZ}" > /etc/timezone \
   && dpkg-reconfigure -f noninteractive tzdata

ENV COMPOSER_ALLOW_SUPERUSER=1

RUN apt-get update && apt-get install -y \
    libmcrypt-dev \
    unzip \
    openssl \
    git \
    unzip \
    zlib1g-dev \
    vim

# enabled mod-rewrite
RUN cd /etc/apache2/mods-enabled \
    && ln -s ../mods-available/rewrite.load

COPY ./000-default.conf /etc/apache2/sites-available/

RUN git clone https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install \
    mbstring \
    pdo \
    pdo_mysql \
    redis \
    zip \
    pcntl

# install xdebug
RUN pecl install xdebug
RUN docker-php-ext-enable xdebug

# php ini
COPY ./docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/docker-php-ext-xdebug.ini
COPY ./php.ini /usr/local/etc/php/php.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# packagist.jp add
RUN composer config -g repos.packagist composer https://packagist.jp
RUN composer global require hirak/prestissimo

RUN composer global require "laravel/installer"

RUN export PATH=$HOME/.composer/vendor/bin:$PATH

WORKDIR /var/www/html
