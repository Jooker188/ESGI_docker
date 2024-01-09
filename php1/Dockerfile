# Utilisation de l'image PHP avec Composer
FROM php:8.3.0-fpm

# Installer les dépendances nécessaires
RUN apt-get update && apt-get upgrade -y \
    && echo "Installing dependencies..." \
    && apt-get install -y \
        git \
        unzip \
        libzip-dev \
        libonig-dev \
    && echo "Installing PHP extensions..." \
    && docker-php-ext-install pdo_mysql zip \
    && echo "Installing Xdebug..." \
    && pecl install xdebug \
    && docker-php-ext-enable xdebug


# Installer Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Installer Node.js et npm
RUN curl -sL https://deb.nodesource.com/setup_18.x | bash - \
    && apt-get install -y nodejs

# Définir le répertoire de travail
WORKDIR /var/www/html

# Copier les fichiers de l'application Laravel
COPY composer.json .

# Copier le fichier artisan
COPY artisan .

COPY bootstrap .

# Installer les dépendances de l'application
RUN composer install

# Exposer le port PHP-FPM
EXPOSE 9000

# Exécuter les commandes Artisan
RUN php artisan key:generate
RUN php artisan migrate:fresh --seed

CMD ["php-fpm"]