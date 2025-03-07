# Stage 1: Builder
FROM php:7.4-fpm AS builder

RUN apt-get update && apt-get install -y \
    zip unzip git curl libonig-dev mariadb-client \
    && docker-php-ext-install pdo pdo_mysql \
    && docker-php-ext-enable pdo_mysql

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/html

COPY composer.json composer.lock ./

RUN composer install --no-dev --optimize-autoloader

# Stage 2: Production
FROM php:7.4-fpm AS production

WORKDIR /var/www/html

COPY docker/php-fpm/php.ini /usr/local/etc/php/php.ini

COPY --from=builder /var/www/html /var/www/html

RUN chown -R www-data:www-data /var/www/html \
    && chmod -R 755 /var/www/html

CMD ["php-fpm"]
