FROM php:8-fpm

# Install system dependencies
RUN apt-get update && apt-get install -y \
    openssl \
    nodejs \
    npm \
    wget \
    git \
    curl \
    libpng-dev \
    libonig-dev \
    libxml2-dev \
    libzip-dev \
    zip \
    unzip \
    libssl-dev \
    libc-client-dev \
    libkrb5-dev \
    supervisor

# Install PHP extensions
RUN docker-php-ext-install pdo_mysql mbstring exif pcntl bcmath gd

# install swoole
RUN apt-get install -y libssl-dev && yes | pecl install swoole && docker-php-ext-enable swoole.so;

# Install Composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# Enable and install node packages
RUN npm config set unsafe-perm true
RUN npm install

# Added supervisor config
COPY docker/php/supervisord.conf /etc/supervisor/conf.d/supervisord.conf
CMD ["/usr/bin/supervisord", "-n", "-c", "/etc/supervisor/conf.d/supervisord.conf"]

# Set working directory
WORKDIR /var/www

