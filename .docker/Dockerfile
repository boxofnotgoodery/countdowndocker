FROM  php:7.1-apache

RUN apt-get update && \
    apt-get install -y -qq git \
        libjpeg62-turbo-dev \
        apt-transport-https \
        libfreetype6-dev \
        libmcrypt-dev \
        libpng-dev \
        libssl-dev \
        zip unzip \
        nodejs \
        npm \
        wget \
        vim

RUN pecl install redis && docker-php-ext-enable redis
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/
RUN docker-php-ext-install -j$(nproc) iconv mcrypt zip pdo pdo_mysql gd bcmath

COPY .docker/vhost.conf /etc/apache2/sites-available/000-default.conf
COPY . /app

RUN chown -R www-data:www-data /app && a2enmod rewrite
RUN for mod in rewrite headers; do a2enmod $mod; done && service apache2 restart

WORKDIR /app/