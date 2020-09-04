FROM php:7.3-cli-alpine

WORKDIR /usr/src/myapp

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories; \
    apk add --no-cache tzdata; \
    ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime; \
    echo "Asia/Shanghai" > /etc/timezone;

RUN mv "$PHP_INI_DIR/php.ini-production" "$PHP_INI_DIR/php.ini"; \
    php -v;

RUN apk upgrade --update \
    && apk add --no-cache --virtual .php-deps \
    autoconf \
    libc-dev \
    make \
    musl-dev \
    re2c \
    bison \
    libtool \
    zlib-dev \
    libmemcached-dev \
    libmcrypt-dev \
    yaml-dev \
    libxml2-dev \
    libzip-dev \
    icu-dev \
    libpng-dev \
    libxslt-dev \
    && rm -rf /tmp/* /var/cache/apk/*

RUN apk add --no-cache --virtual .build-deps gcc g++ \
    && pecl install memcached \
    && docker-php-ext-enable memcached \
    && pecl install mcrypt \
    && docker-php-ext-enable mcrypt \
    && pecl install redis \
    && docker-php-ext-enable redis \
    && pecl install yaml \
    && docker-php-ext-enable yaml \
    && pecl install mongodb \
    && docker-php-ext-enable mongodb \
    && pecl install inotify \
    && docker-php-ext-enable inotify \
    && pecl install swoole \
    && docker-php-ext-enable swoole \
    && apk del .build-deps gcc g++ \
    && rm -rf /tmp/* /var/cache/apk/*

RUN docker-php-ext-configure intl \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-enable intl

RUN docker-php-ext-install -j$(nproc) bcmath \
    pdo_mysql \
    mysqli \
    xmlrpc \
    tokenizer \
    zip \
    opcache \
    pcntl \
    gd \
    soap \
    xsl

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"; \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer; \
    php -r "unlink('composer-setup.php');";

EXPOSE 9000-9050