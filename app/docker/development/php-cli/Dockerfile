FROM php:8.1-cli-alpine

#ENV XDEBUG_VERSION 3.2.0

RUN set -eux  \
    && apk add --no-cache --update linux-headers postgresql-dev bash coreutils git \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
#    && git clone --branch $XDEBUG_VERSION --depth 1 https://github.com/xdebug/xdebug.git /usr/src/php/ext/xdebug \
#    && docker-php-ext-configure xdebug --enable-xdebug-dev \
    && docker-php-ext-install sockets pdo_pgsql intl \
    && apk del git

RUN apk add --no-cache unzip

RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini

COPY ./common/php/conf.d /usr/local/etc/php/conf.d
COPY ./development/php/conf.d /usr/local/etc/php/conf.d

COPY ./common/wait-for-it.sh /usr/local/bin/wait-for-it
RUN chmod 555 /usr/local/bin/wait-for-it

ENV COMPOSER_ALLOW_SUPERUSER 1

#COPY --from=composer:2 /usr/bin/composer /usr/local/bin/composer
COPY --from=composer/composer:latest-bin /composer /usr/bin/composer

#RUN addgroup -g 1000 app && adduser -u 1000 -G app -s /bin/sh -D app

WORKDIR /app

#RUN mkdir /app/vendor
#RUN chmod -R 755 /app/vendor
#RUN chown -R app:app /app/vendor
#USER app

