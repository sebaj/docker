FROM php:8.4-apache

# Herramientas de build + headers necesarios
RUN apt-get update && apt-get -y upgrade && apt-get install -y \
    ca-certificates curl unzip git pkg-config \
    build-essential autoconf \
    libicu-dev \
    libzip-dev zlib1g-dev \
    libfreetype6-dev libjpeg62-turbo-dev libpng-dev libwebp-dev \
    libonig-dev \
 && rm -rf /var/lib/apt/lists/*

# --- GD ---
RUN set -eux; \
    docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
    || (echo "=== gd config.log ===" && cat /usr/src/php/ext/gd/config.log || true; exit 1); \
    docker-php-ext-install -j"$(nproc)" gd

# --- INTL ---
RUN set -eux; \
    docker-php-ext-configure intl \
    || (echo "=== intl config.log ===" && cat /usr/src/php/ext/intl/config.log || true; exit 1); \
    docker-php-ext-install -j"$(nproc)" intl

# --- ZIP ---
RUN set -eux; \
    docker-php-ext-configure zip \
    || (echo "=== zip config.log ===" && cat /usr/src/php/ext/zip/config.log || true; exit 1); \
    docker-php-ext-install -j"$(nproc)" zip

# --- MYSQLI & PDO_MYSQL ---
RUN docker-php-ext-install -j"$(nproc)" mysqli pdo_mysql

# --- MBSTRING (requiere libonig-dev) ---
RUN docker-php-ext-install -j"$(nproc)" mbstring

# Opcional: OPcache
RUN docker-php-ext-install opcache

# Copiar el archivo de configuración personalizado de PHP
COPY docker-php-prestashop.ini /usr/local/etc/php/conf.d/docker-php-prestashop.ini

# Apache
RUN a2enmod rewrite

WORKDIR /var/www/html