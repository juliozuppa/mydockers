FROM php:7.2.10-apache
LABEL maintainer="juliozuppa@gmail.com"

ENV COMPOSER_ALLOW_SUPERUSER=1

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    libpng-dev \
    libicu-dev \
    libzip-dev \
    libgd-dev \
    libpq-dev \
    libxml2-dev && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-install bcmath gd gettext intl pdo_pgsql pgsql soap zip opcache && \
    docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \
    docker-php-ext-configure hash --with-mhash && \
    docker-php-ext-configure intl && \
    docker-php-ext-configure zip --with-libzip && \
    docker-php-ext-configure opcache && \
    pecl install -f xdebug-2.9.0 && \
    docker-php-ext-enable xdebug && \
    a2enmod rewrite && \
    rm -rf /tmp/* /var/tmp/* /usr/share/doc/* /usr/src/* && \
    echo "memory_limit=-1" > $PHP_INI_DIR/conf.d/memory-limit.ini && \
    echo "error_reporting = E_ALL & ~E_DEPRECATED & ~E_STRICT & ~E_WARNING & ~E_NOTICE" > $PHP_INI_DIR/conf.d/error-reporting.ini && \
    echo "date.timezone=America/Sao_Paulo" > $PHP_INI_DIR/conf.d/date_timezone.ini

COPY configs/apache2.conf /etc/apache2/
COPY configs/000-default.conf /etc/apache2/sites-enabled/
WORKDIR "/var/www"
EXPOSE 80