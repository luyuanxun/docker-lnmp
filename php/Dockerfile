FROM php:7.2-fpm

#mirrors.163.com
COPY ./sources.list.stretch /etc/apt/sources.list
# RUN rm /etc/apt/sources.list.d/buster.list

#ext package
RUN chmod -R 777 /tmp
ADD . /tmp

MAINTAINER luyuanxun <luyuanxun@qq.com>

# Version
ENV PHPREDIS_VERSION 4.1.1
ENV HIREDIS_VERSION 0.13.3
ENV SWOOLE_VERSION 4.1.0

# Timezone
RUN /bin/cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo 'Asia/Shanghai' > /etc/timezone

RUN apt-get update \
    && apt-get install -y \
        curl \
        wget \
        git \
        zip \
        libz-dev \
        libssl-dev \
        libnghttp2-dev \
        libjpeg-dev \
        libpng-dev \
        libfreetype6-dev \
        libnghttp2-dev \
        libpcre3-dev \
    && apt-get clean \
    && apt-get autoremove

# mysqli extension
RUN docker-php-ext-install mysqli
# PDO extension
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && docker-php-ext-install -j$(nproc) gd

# Composer
RUN mv /tmp/composer.phar /usr/local/bin/composer \
    && chmod a+x /usr/local/bin/composer \
    && composer self-update --clean-backups

# Redis extension
RUN cp /tmp/ext/redis-${PHPREDIS_VERSION}.tgz redis.tar.tgz \
    && pecl install redis.tar.tgz \
    && rm -rf redis.tar.tgz \
    && docker-php-ext-enable redis

# Hiredis
RUN cp /tmp/ext/hiredis-${HIREDIS_VERSION}.tar.gz hiredis.tar.gz \
    && mkdir -p hiredis \
    && tar -xf hiredis.tar.gz -C hiredis --strip-components=1 \
    && rm hiredis.tar.gz \
    && ( \
        cd hiredis \
        && make -j$(nproc) \
        && make install \
        && ldconfig \
    ) \
    && rm -r hiredis

# Swoole extension
RUN cp /tmp/ext/swoole-src-${SWOOLE_VERSION}.tar.gz swoole.tar.gz \
    && mkdir -p swoole \
    && tar -xf swoole.tar.gz -C swoole --strip-components=1 \
    && rm swoole.tar.gz \
    && ( \
        cd swoole \
        && phpize \
        && ./configure --enable-async-redis --enable-mysqlnd --enable-openssl --enable-http2 \
        && make -j$(nproc) \
        && make install \
    ) \
    && rm -r swoole \
    && docker-php-ext-enable swoole

RUN rm -rf /tmp/ext
