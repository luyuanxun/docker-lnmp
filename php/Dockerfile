FROM php:7.3.4-fpm-alpine3.9

MAINTAINER luyuanxun <luyuanxun@qq.com>

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories
RUN apk add --update-cache bash

# --build-arg timezone=Asia/Shanghai
ARG timezone
ENV TIMEZONE=${timezone:-"Asia/Shanghai"}

RUN ln -sf /usr/share/zoneinfo/${TIMEZONE} /etc/localtime \
        && echo "${TIMEZONE}" > /etc/timezone

# extension
RUN docker-php-ext-install pdo_mysql
RUN apk upgrade --update && apk add \
            freetype-dev \
            libjpeg-turbo-dev \
            libpng-dev \
        && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
        && docker-php-ext-install -j$(nproc) gd

WORKDIR /var/www