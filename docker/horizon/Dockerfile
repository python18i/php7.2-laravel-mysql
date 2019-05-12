FROM php:7.1-cli

ENV TZ Asia/Tokyo
RUN echo "${TZ}" > /etc/timezone \
   && dpkg-reconfigure -f noninteractive tzdata

RUN apt-get update && apt-get install -y \
    unzip \
    git \
    cron \
    supervisor \
    vim

RUN git clone https://github.com/phpredis/phpredis.git /usr/src/php/ext/redis

# Type docker-php-ext-install to see available extensions
RUN docker-php-ext-install mysqli pdo_mysql redis pcntl

ADD ./cron.d /etc/cron.d/
RUN chmod 0644 /etc/cron.d/*

ADD ./php.ini /usr/local/etc/php/php.ini
COPY ./supervisord.conf /etc/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisord.conf"]
