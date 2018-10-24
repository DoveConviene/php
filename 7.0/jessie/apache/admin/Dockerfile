FROM doveconviene/php:7.0.31-apache

# Set user 1000 and group staff to www-data, enables write permission.
# https://github.com/boot2docker/boot2docker/issues/581#issuecomment-114804894
RUN usermod -u 1000 www-data
RUN usermod -G staff www-data

# Install modules
RUN apt-get update && apt-get install -y libicu-dev zlib1g-dev libxml2-dev && rm -rf /var/lib/apt/lists/* \
	&& docker-php-ext-install zip \
    && docker-php-ext-install intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-install pdo_mysql

RUN apt-get update && apt-get install -y libmcrypt-dev \
    && docker-php-ext-install mcrypt

RUN apt-get update && apt-get install -y libmemcached-dev zlib1g-dev \
    && pecl install memcached-3.0.3 \
    && docker-php-ext-enable memcached

RUN docker-php-source extract

RUN cd /tmp/ && \
	curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/3.1.0.tar.gz && \
    tar xf redis.tar.gz && \
		rm -f redis.tar.gz && \
    mv phpredis-3.1.0 /usr/src/php/ext/redis && \
    docker-php-ext-install redis

RUN cd /tmp/ && \
    curl -O https://pecl.php.net/get/xdebug-2.5.1.tgz && \
    tar xf xdebug-2.5.1.tgz && \
		rm -f xdebug-2.5.1.tgz && \
    mv xdebug-2.5.1 /usr/src/php/ext/xdebug && \
    docker-php-ext-install xdebug

RUN docker-php-source delete

# needed for php code sniffer fixer (phpcbf)
RUN apt-get update && apt-get install patch

RUN echo "date.timezone = \"Europe/Rome\"" > /usr/local/etc/php/conf.d/date.ini

RUN a2enmod rewrite

WORKDIR /var/www/html

EXPOSE 80
CMD ["apache2-foreground"]