FROM php:8.1-fpm

RUN curl -sL https://deb.nodesource.com/setup_18.x | bash -

RUN apt-get update \
    && apt-get install -y vim unzip git wget nodejs \
    libzip-dev libpq-dev libonig-dev libmcrypt-dev mariadb-client \
    libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    vim less \
    && docker-php-ext-install zip pdo_mysql pdo_mysql mysqli mbstring iconv \
    # && docker-php-ext-install zip pdo_mysql pdo_mysql mysqli mbstring iconv opcache \
    && docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd exif

COPY define/php/php.ini /usr/local/etc/php

# Composer install
COPY --from=composer /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

RUN apt-get install -y nginx
RUN rm -rf /etc/nginx/sites-enabled/default
COPY define/nginx/nginx.conf /etc/nginx

RUN apt-get install -y tzdata && \
    ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime

#COPY define/cron/root /var/spool/cron/crontabs/
#RUN chmod 600 /var/spool/cron/crontabs/root
RUN echo -e "\e[37;42;1m\n\n  INSTALL cron\n\e[m" \
    && apt-get install -y --no-install-recommends cron
# COPY define/cron/cron /etc/cron.d/cron
# RUN chmod 644 /etc/cron.d/cron
# RUN crontab /etc/cron.d/cron

RUN mkdir /var/log/laravel
RUN mkdir /var/log/cron

RUN apt-get -y install supervisor

COPY define/nginx/default.conf /etc/nginx/sites-enabled/
#COPY define/php/www.conf /usr/local/etc/php-fpm.d
COPY define/supervisor/supervisord.conf /etc/supervisord.conf
COPY define/supervisor/conf.d /etc/supervisord.d

RUN mkdir share-images
COPY ./share-images /var/www/share-images
RUN cd /var/www/share-images ; chmod 777 -R storage
WORKDIR /var/www/share-images
CMD ["/usr/bin/supervisord", "-c",  "/etc/supervisord.conf"]