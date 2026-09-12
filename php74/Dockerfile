FROM php:7.4-fpm


RUN apt-get update && apt-get install -y \
    libzip-dev libpng-dev libonig-dev libxml2-dev unzip git curl \
    libfreetype6-dev libjpeg62-turbo-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install pdo_mysql zip gd mbstring xml bcmath
    
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www
