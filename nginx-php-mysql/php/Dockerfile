FROM php:fpm
COPY php.ini /usr/local/etc/php/

# vim install
RUN apt-get update
RUN apt-get install -y vim

# php module
RUN apt-get update && apt-get install -y \
   zlib1g-dev \
   git \
   libzip* \
   libonig-dev \
   libc-client-dev \
   libkrb5-dev \
   libxml2-dev 

RUN rm -r /var/lib/apt/lists/*

RUN PHP_OPENSSL=yes docker-php-ext-configure imap --with-kerberos --with-imap-ssl \
    && docker-php-ext-install zip pdo_mysql mysqli mbstring imap soap


# composer install
COPY --from=composer /usr/bin/composer /usr/bin/composer
