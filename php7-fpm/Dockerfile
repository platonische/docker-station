# See https://github.com/docker-library/php/blob/master/7.4/fpm/Dockerfile
FROM php:7.4-fpm
ARG TIMEZONE

MAINTAINER Shliakhov Sergey <shlyakhov.up@gmail.com>

RUN apt-get update && apt-get install -y \
    openssl \
    git \
    unzip \
    apt-utils \
    libpng-dev



# Install Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer --version

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

RUN apt-get update && apt-get install -y libmcrypt-dev mariadb-client
RUN pecl install mcrypt \
    && docker-php-ext-enable mcrypt
#RUN docker-php-ext-configure mcrypt

# Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install gd



RUN apt-get update && apt-get install -y procps

RUN usermod -u 1000 www-data

WORKDIR /var/www/app
RUN chown -R www-data:www-data /var/www/app
USER www-data

#RUN git clone https://github.com/laravel/laravel.git .

#RUN composer install