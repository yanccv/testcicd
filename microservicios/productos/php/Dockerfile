# Partimos de la imagen php en su versión 7.4
FROM php:8.2-fpm

# Nos movemos a /var/www/
WORKDIR /var/www

# Instalamos las dependencias necesarias
RUN apt-get update && apt-get install -y \
    libfreetype-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    build-essential \
    libzip-dev \
    #libpng-dev \
    #libjpeg62-turbo-dev \
    #libfreetype6-dev \
    libonig-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    git \
    curl

# Instalamos extensiones de PHP
RUN docker-php-ext-install pdo_mysql zip exif pcntl
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd

# Instalamos composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

#RUN addgroup -S www-data && adduser -SH www-data www-data


RUN chmod -R 775 /var/www
RUN chown -R www-data:www-data /var/www
RUN rm -rf html
USER www-data
RUN composer create-project laravel/laravel html && cd html && php artisan install:api --quiet
# Instalamos dependendencias de composer
#RUN composer install --no-ansi --no-dev --no-interaction --no-progress --optimize-autoloader --no-scripts

# Copiamos todos los archivos de la carpeta actual de nuestra 
# computadora (los archivos de laravel) a /var/www/

# Exponemos el puerto 9000 a la network
EXPOSE 9000

# Corremos el comando php-fpm para ejecutar PHP
CMD ["php-fpm"]