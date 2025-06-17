FROM php:8.4-apache

RUN apt-get update && apt-get -y upgrade

RUN apt-get install -y \
		curl \
		gnupg \
		libicu-dev \
		libzip-dev \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libwebp-dev \
		libpng-dev \
		unzip

RUN docker-php-ext-install intl zip pdo_mysql \
		&& docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp \
		&& docker-php-ext-install -j$(nproc) gd

RUN curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
    && apt-get install -y nodejs

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

COPY docker-php-prestashop.ini /usr/local/etc/php/conf.d/

WORKDIR /var/www/html/
COPY ./src/* .
RUN unzip *.zip \
	&& rm *.zip \
	&& mv */* . \
	&& rm -rf PrestaShop*

RUN chown -R www-data:www-data .
 
RUN a2enmod rewrite
RUN service apache2 restart
