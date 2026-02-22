FROM php:8.3-fpm-alpine

# --------
# Args para mapear UID/GID del host (ideal para Linux/WSL2)
# --------
ARG PUID=1000
ARG PGID=1000

# Dependencias del sistema
RUN apk add --no-cache \
    bash curl git unzip \
    icu-dev oniguruma-dev \
    libzip-dev \
    mysql-client \
    $PHPIZE_DEPS

# Extensiones PHP necesarias para Laravel
RUN docker-php-ext-install \
    pdo_mysql \
    mbstring \
    intl \
    zip \
    opcache

# Composer
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# php.ini custom
COPY docker/php/php.ini /usr/local/etc/php/conf.d/custom.ini

# --------
# Crear grupo/usuario no-root con UID/GID del host
# --------
RUN addgroup -g ${PGID} laravel \
    && adduser  -D -G laravel -u ${PUID} laravel

WORKDIR /var/www/html

# Directorios que Laravel necesita escribir (en runtime)
# Nota: como montas volumen, esto ayuda cuando no hay volumen o en CI.
RUN mkdir -p /var/www/html/storage /var/www/html/bootstrap/cache \
    && chown -R laravel:laravel /var/www/html

# Composer sin warnings
ENV COMPOSER_ALLOW_SUPERUSER=0

# Correr como usuario no-root
USER laravel

# php-fpm ya viene como CMD por defecto en la imagen base