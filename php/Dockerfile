FROM php:8.3-fpm

LABEL org.opencontainers.image.authors="Shawn Leske <admin@shawnleske.de>"

RUN set -eux \
	&& groupadd -g 1000 -r phpuser \
	&& useradd -u 1000 -m -s /bin/bash -g phpuser phpuser

RUN set -eux \
	&& rm -rf /usr/local/etc/php-fpm.d \
	&& mkdir -p /usr/local/etc/php-fpm.d \
	&& mkdir -p /var/lib/php/session \
	&& mkdir -p /var/lib/php/wsdlcache \
	&& chown -R phpuser:phpuser /var/lib/php/session \
	&& chown -R phpuser:phpuser /var/lib/php/wsdlcache

COPY php.ini /usr/local/etc/php/conf.d/docker-php-config.ini
COPY php-fpm.conf /usr/local/etc/php-fpm.conf

RUN apt-get update && apt-get install -y \
    gnupg \
    g++ \
    procps \
    openssl \
    git \
    unzip \
    zlib1g-dev \
    libzip-dev \
    libfreetype6-dev \
    libpng-dev \
    libjpeg-dev \
    libicu-dev  \
    libonig-dev \
    libxslt1-dev \
    acl 

RUN docker-php-ext-configure gd --with-jpeg --with-freetype 

RUN docker-php-ext-install \
    pdo pdo_mysql zip xsl gd intl opcache exif mbstring

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN curl -sS https://get.symfony.com/cli/installer | bash
RUN mv /root/.symfony5/bin/symfony /usr/local/bin/symfony

EXPOSE 9000