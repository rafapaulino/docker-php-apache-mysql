FROM php:8.0-apache

# 1. Install development packages and clean up apt cache.
RUN apt-get update && apt-get install -y \
    curl \
    g++ \
    git \
    libbz2-dev \
    libfreetype6-dev \
    libicu-dev \
    libjpeg-dev \
    libmcrypt-dev \
    libpng-dev \
    libreadline-dev \
    sudo \
    unzip \
    zip \
    libonig-dev \
 && rm -rf /var/lib/apt/lists/*


COPY ./000-default.conf /etc/apache2/sites-available/000-default.conf

RUN a2enmod rewrite headers &&\
    a2dissite 000-default &&\
    a2ensite 000-default &&\
    service apache2 restart


# 4. Start with base PHP config, then add extensions.
RUN mv "$PHP_INI_DIR/php.ini-development" "$PHP_INI_DIR/php.ini"

RUN docker-php-ext-install \
    bcmath \
    bz2 \
    calendar \
    iconv \
    intl \
    mbstring \
    opcache \
    pdo_mysql 




# Install composer
ENV PATH ./vendor/bin:/composer/vendor/bin:$PATH
ENV COMPOSER_ALLOW_SUPERUSER 1
RUN curl -s https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

# Install PHP_CodeSniffer
RUN composer global require "squizlabs/php_codesniffer=*"

ENV PATH="/root/.composer/vendor/squizlabs/php_codesniffer/bin/:${PATH}"

# Instalação e configuração do XDebug
RUN pecl install xdebug && docker-php-ext-enable xdebug


EXPOSE 80