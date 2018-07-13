FROM php:7.2-apache

RUN apt-get update && apt-get install -y \
  git \
  vim \
  zip \
  zlib1g-dev

RUN docker-php-ext-install zip

RUN a2enmod rewrite

RUN curl -sS https://getcomposer.org/installer | \
  php -- --install-dir=/usr/local/bin --filename=composer

COPY ./conf/000-default.conf /etc/apache2/sites-available/
