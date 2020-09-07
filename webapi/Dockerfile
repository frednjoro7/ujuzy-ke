FROM php:7.2-fpm

LABEL app_name="ujuzy-backend"

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libzip-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-configure gd --with-gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ --with-png-dir=/usr/include/
RUN docker-php-ext-configure zip --with-libzip
RUN docker-php-ext-install pdo_mysql mbstring zip gd exif pcntl sockets bcmath

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer \
    &&  composer global require hirak/prestissimo

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

WORKDIR /var/www

# Copy existing application directory contents
COPY . /var/www

# Copy existing application directory permissions
RUN chown -R www /var/www

# Change current user to www
USER www

# install php dependencies
RUN composer install --no-dev --no-progress --profile --prefer-dist
RUN php artisan key:generate

# Expose port 9000 and start php-fpm server
EXPOSE 9000