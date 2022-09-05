FROM php:8.1-fpm-buster
ARG TIMEZONE

COPY php.ini /usr/local/etc/php/conf.d/docker-php-config.ini
COPY php_pdo_mysql.dll /usr/local/etc/php/conf.d/php_pdo_mysql.dll

RUN apt-get update && apt-get install -y \
    gnupg \
    g++ \
    procps \
    openssl \
    git \
    sudo \
    curl \
    unzip \
    zlib1g-dev \
    libzip-dev \
    libfreetype6-dev \
    libpng-dev \
    libjpeg-dev \
    libicu-dev  \
    libonig-dev \
    libxslt1-dev \
    acl \
    && echo 'alias sf="php bin/console"' >> ~/.bashrc

RUN sudo apt-get install curl
RUN curl -fsSL https://deb.nodesource.com/setup_17.x | bash -
RUN sudo apt-get install -y nodejs


RUN docker-php-ext-configure gd --with-jpeg --with-freetype 

RUN docker-php-ext-install \
    pdo pdo_mysql zip xsl gd intl opcache exif mbstring

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone \
    && printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini \
    && "date"

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

WORKDIR /var/www/symfony
