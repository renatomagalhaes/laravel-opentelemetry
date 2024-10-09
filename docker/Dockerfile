FROM php:8.1-fpm

# Instalar extensões necessárias
RUN apt-get update && apt-get install -y \
    git \
    unzip \
    libzip-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install pdo pdo_mysql gd zip

# Instalar Xdebug
RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Configurações do Xdebug para ambiente local
COPY ./docker/php/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Instalar OpenTelemetry para PHP
RUN curl -sSfL https://github.com/open-telemetry/opentelemetry-php/releases/download/v1.0.0/opentelemetry.so -o /usr/local/lib/php/extensions/no-debug-non-zts-20210902/opentelemetry.so \
    && echo "extension=opentelemetry.so" > /usr/local/etc/php/conf.d/opentelemetry.ini

# Definir o fuso horário
RUN ln -snf /usr/share/zoneinfo/America/Sao_Paulo /etc/localtime && echo "America/Sao_Paulo" > /etc/timezone

# Definir o diretório de trabalho
WORKDIR /var/www/html

# Instalar Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Copiar arquivos do Laravel
COPY . /var/www/html

# Instalar dependências do Laravel
RUN composer install