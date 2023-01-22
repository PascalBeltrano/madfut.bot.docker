FROM php:8.2-apache

# Installer les dépendances de PHP
RUN apt-get update && apt-get install -y \
        git \
        gnupg \
        libfreetype6-dev \
        libicu-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
        libzip-dev \
        unzip \
        zip \
        zlib1g-dev \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install zip

# Remplacer le fichier de conf php    
COPY conf/php.ini /usr/local/etc/php/conf.d/app.ini

# Installer composer
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Remplacer les fichiers de conf apache
COPY conf/apache.conf /etc/apache2/conf-available/z-app.conf
COPY conf/vhost.conf /etc/apache2/sites-available/000-default.conf
COPY errors /errors

# Activer mod_rewrite d'Apache
RUN a2enmod rewrite && a2enconf z-app

# Exposer le port 80
EXPOSE 80
WORKDIR /app

# Permission de droits pour le cache
RUN chmod -R 777 /app
