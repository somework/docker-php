FROM alpine:3.8

MAINTAINER Igor Pinchuk <i.pinchuk.work@gmail.com>

ENV LC_ALL ${LC_ALL:-"ru_RU.UTF-8"}
ENV LANG ${LANG:-"ru_RU.UTF-8"}

ENV TIMEZONE ${TIMEZONE:-"Europe/Moscow"}


ENV COMPOSER_ALLOW_SUPERUSER 1


ENV ENV_DEPS="git \
            openssl \
            unzip \
            curl \
            openssh-client \
            icu \
            libpng \
            libjpeg-turbo \
            ca-certificates"

ENV PHP_DEPS="php7.2 \
              php7.2-fpm \
              php7.2-opcache \
              php7.2-ctype \
              php7.2-json \
              php7.2-dom \
              php7.2-zip \
              php7.2-gd \
              php7.2-curl \
              php7.2-mbstring \
              php7.2-redis \
              php7.2-mcrypt \
              php7.2-iconv \
              php7.2-posix \
              php7.2-pdo \
              php7.2-pdo_mysql \
              php7.2-mysqli \
              php7.2-tokenizer \
              php7.2-simplexml \
              php7.2-session \
              php7.2-xml \
              php7.2-sockets \
              php7.2-openssl \
              php7.2-fileinfo \
              php7.2-exif \
              php7.2-pcntl \
              php7.2-xmlwriter \
              php7.2-phar \
              php7.2-zlib \
              php7.2-intl"

RUN addgroup -g 1000 -S alpine-www-data \
    && adduser -u 1000 -D -S -G alpine-www-data alpine-www-data

RUN set -x \
    && apk add --no-cache $ENV_DEPS




# PHP
# PHP.earth Alpine repository for better developer experience
ADD https://repos.php.earth/alpine/phpearth.rsa.pub /etc/apk/keys/phpearth.rsa.pub
RUN echo "https://repos.php.earth/alpine/v3.8" >> /etc/apk/repositories

RUN set -x \
    && apk add --no-cache $PHP_DEPS

ADD php-fpm.conf /etc/php/7.2/
ADD zz-docker.conf /etc/php/7.2/php-fpm.d/
ADD docker.conf /etc/php/7.2/php-fpm.d/
RUN rm /etc/php/7.2/php-fpm.d/www.conf

# Set timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', `echo $TIMEZONE` > /etc/php/7.2/conf.d/tzone.ini

# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Install project ini
COPY project.ini /etc/php/7.2/conf.d/

RUN mkdir -p /var/log/php/7.2/ \
    && chown alpine-www-data:alpine-www-data /var/log/php/7.2/

RUN rm -rf /tmp/* \
    && rm -rf /var/cache/apk/*

WORKDIR /var/www

USER alpine-www-data

EXPOSE 9000
CMD ["php-fpm"]