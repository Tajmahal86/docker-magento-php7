FROM php:7.2-fpm-alpine

MAINTAINER Javier Telio Zapot <jtelio118@gmail.com>

WORKDIR /var/www/html

COPY . .

# Install Script's
COPY ./bin/install-magento /usr/local/bin/install-magento
RUN chmod +x /usr/local/bin/install-magento

COPY ./sampledata/magento-sample-data-1.9.1.0.tgz /opt/
COPY ./bin/install-sampledata-1.9 /usr/local/bin/install-sampledata
RUN	chmod +x /usr/local/bin/install-sampledata

COPY ./bin/init /usr/local/bin/init
RUN chmod +x /usr/local/bin/init

# Install Libs
RUN apk add --no-cache --virtual .build-deps \
	$PHPIZE_DEPS \
	libxml2-dev \
	libpng \
	libpng-dev \
	libmcrypt-dev \
	jpeg-dev \
	libjpeg-turbo-dev \
	freetype-dev \
	mysql-client \
	imagemagick-dev

# Install GD
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/

# Install PHP Ext
RUN docker-php-ext-install soap \
	pdo_mysql \
	mbstring \
	gd \
	iconv \
	pcntl \
	posix \
	bcmath \
	zip \
	sockets


RUN pecl install imagick && \
	docker-php-ext-enable imagick

# Download the latest version of modman
RUN curl -SL https://raw.githubusercontent.com/colinmollenhour/modman/master/modman -o modman \
    && chmod +x ./modman \
    && mv ./modman /usr/local/bin/

#COPY redis.conf /var/www/htdocs/app/etc/

#Expose port
EXPOSE 9000

ENTRYPOINT ["/bin/sh", "/usr/local/bin/init"]
