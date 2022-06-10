FROM php:8.0.5-fpm
ARG TIMEZONE
#Dockerfile
RUN apt-get update \
    && apt-get install -y libpq-dev git libicu-dev libxml2-dev supervisor \
    libzip-dev \
    librabbitmq-dev \
    libssh-dev \
    zip \
    && rm -rf /var/lib/apt/lists/* \
    && pecl install amqp \
    && docker-php-ext-enable amqp

RUN docker-php-ext-configure intl \
    && docker-php-ext-install \
        intl \
        zip \
        bcmath \
        sockets \
        soap \
        pcntl \
        opcache \
        pdo \
        pdo_mysql

RUN curl --insecure https://getcomposer.org/composer.phar -o /usr/bin/composer && chmod +x /usr/bin/composer

# Set timezone
RUN ln -snf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo ${TIMEZONE} > /etc/timezone
RUN printf '[PHP]\ndate.timezone = "%s"\n', ${TIMEZONE} > /usr/local/etc/php/conf.d/tzone.ini
RUN "date"

COPY docker/php-fpm/entry_point.sh /opt/bin/
RUN ["chmod", "+x", "/opt/bin//entry_point.sh"]

WORKDIR /var/www/html

CMD ["/opt/bin/entry_point.sh"]

RUN chown -R 1000:1000 /var/www/html

RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony/bin/symfony /usr/local/bin/symfony
RUN git config --global user.email "my@email.nl" \
    && git config --global user.name "WEBSITE"

COPY docker/php-fpm/ini/memory_limit.ini /usr/local/etc/php/conf.d
