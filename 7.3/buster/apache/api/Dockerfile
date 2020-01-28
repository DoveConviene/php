FROM doveconviene/php:7.3.11-apache
# Set user 1000 and group staff to www-data, enables write permission.
# https://github.com/boot2docker/boot2docker/issues/581#issuecomment-114804894
RUN usermod -u 1000 www-data
RUN usermod -G staff www-data
# Install modules
RUN apt-get update && apt-get install -y libicu-dev zlib1g-dev libzip-dev libxml2-dev && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install zip \
    && docker-php-ext-install intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install soap
RUN docker-php-source extract
RUN cd /tmp/ && \
    curl -O https://pecl.php.net/get/xdebug-2.9.1.tgz && \
    tar xf xdebug-2.9.1.tgz && \
        rm -f xdebug-2.9.1.tgz && \
    mv xdebug-2.9.1 /usr/src/php/ext/xdebug && \
    docker-php-ext-install xdebug
RUN docker-php-source delete
# to deprecate in the very next future
RUN apt-get update && apt-get install -y default-mysql-client
RUN echo "date.timezone = \"Europe/Rome\"" > /usr/local/etc/php/conf.d/date.ini
RUN a2enmod rewrite
# composer
ENV COMPOSER_URL="https://getcomposer.org/download/1.9.0/composer.phar"
ENV COMPOSER_ALLOW_SUPERUSER=1
RUN php -r "copy('$COMPOSER_URL', '/usr/bin/composer.phar');"
RUN chmod +x /usr/bin/composer.phar
COPY composer-entrypoint /composer-entrypoint
RUN chmod +x /composer-entrypoint
WORKDIR /var/www/html
EXPOSE 80
CMD ["apache2-foreground"]
