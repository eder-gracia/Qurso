# Imagen de PHP para Laravel. Laravel 13 requiere PHP 8.3 o superior.
FROM php:8.3-fpm

# git y unzip los necesita Composer; libzip-dev para la extensión zip.
RUN apt-get update \
    && apt-get install -y --no-install-recommends git unzip libzip-dev \
    && docker-php-ext-install pdo_mysql zip bcmath \
    && rm -rf /var/lib/apt/lists/*

# Composer (gestor de dependencias de PHP), copiado de su imagen oficial.
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html
