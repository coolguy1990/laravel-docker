FROM php:7.1-alpine

MAINTAINER Kunal Mangaraj <mangaraj.kunal@gmail.com>

RUN apk --update add wget \
  curl \
  git \
  build-base \
  libmemcached-dev \
  libmcrypt-dev \
  libxml2-dev \
  zlib-dev \
  autoconf \
  cyrus-sasl-dev \
  libgsasl-dev \
  supervisor

RUN docker-php-ext-install mysqli mbstring pdo pdo_mysql mcrypt tokenizer xml
RUN pecl channel-update pecl.php.net && pecl install memcached && docker-php-ext-enable memcached

RUN rm /var/cache/apk/* \
    && mkdir -p /var/www

COPY supervisord.conf /etc/supervisord.conf

ENTRYPOINT ["/usr/bin/supervisord", "-n", "-c",  "/etc/supervisord.conf"]

COPY ./php71.ini /usr/local/etc/php/php.ini

WORKDIR /etc/supervisor/conf.d/
