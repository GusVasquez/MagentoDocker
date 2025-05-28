FROM php:8.1-fpm-buster

RUN apt-get update && apt-get install -y \
  cron \
  git \
  gzip \
  libbz2-dev \
  libfreetype6-dev \
  libicu-dev \
  libjpeg62-turbo-dev \
  libmcrypt-dev \
  libpng-dev \
  libsodium-dev \
  libssh2-1-dev \
  libxslt1-dev \
  libzip-dev \
  lsof \
  default-mysql-client \
  vim \
  zip \
  libonig-dev \
  gnupg \
  mailutils \
  msmtp 

RUN pecl channel-update pecl.php.net && pecl install \
  redis \
  ssh2-1.3.1 \
  xdebug \
  && pecl clear-cache \
  && rm -rf /tmp/pear

RUN docker-php-ext-configure gd --with-freetype --with-jpeg

RUN docker-php-ext-install \
  bcmath \
  bz2 \
  calendar \
  exif \
  gd \
  gettext \
  intl \
  mysqli \
  opcache \
  pcntl \
  pdo_mysql \
  soap \
  sockets \
  sodium \
  sysvmsg \
  sysvsem \
  sysvshm \
  xsl \
  zip

RUN docker-php-ext-enable \
  redis \
  ssh2 \
  xdebug

# RUN cd /tmp \
#   && curl -O https://downloads.ioncube.com/loader_downloads/ioncube_loaders_lin_x86-64.tar.gz \
#   && tar zxvf ioncube_loaders_lin_x86-64.tar.gz \
#   && export PHP_VERSION=$(php -r "echo PHP_MAJOR_VERSION.'.'.PHP_MINOR_VERSION;") \
#   && export PHP_EXT_DIR=$(php-config --extension-dir) \
#   && cp "./ioncube/ioncube_loader_lin_${PHP_VERSION}.so" "${PHP_EXT_DIR}/ioncube.so" \
#   && rm -rf ./ioncube \
#   && rm ioncube_loaders_lin_x86-64.tar.gz \
#   && docker-php-ext-enable ioncube


RUN curl -sS https://getcomposer.org/installer | \
  php -- --version=2.2.18 --install-dir=/usr/local/bin --filename=composer


COPY conf/www.conf /usr/local/etc/php-fpm.d/
COPY conf/php.ini /usr/local/etc/php/
COPY conf/php-fpm.conf /usr/local/etc/
COPY conf/msmtprc /etc/msmtprc
COPY bin/cronstart /usr/local/bin/
COPY conf/Zscaler_Root_CA.pem /usr/local/etc/

RUN groupadd -g 1000 app \
  && useradd -g 1000 -u 1000 -d /var/www -s /bin/bash app

RUN mkdir -p /etc/nginx/html /var/www/html /sock \
  && chown -R app:app /etc/nginx /var/www /usr/local/etc/php/conf.d /sock

USER app:app
VOLUME /var/www
WORKDIR /var/www/html