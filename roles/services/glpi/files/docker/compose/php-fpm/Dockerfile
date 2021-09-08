FROM alpine:3.13
LABEL maintainer="Ilya Pavlov <piv@devmem.ru>"

ARG S6_OVERLAY_RELEASE=https://github.com/just-containers/s6-overlay/releases/latest/download/s6-overlay-amd64.tar.gz

ENV S6_OVERLAY_RELEASE=${S6_OVERLAY_RELEASE} \
    TZ=Europe/Moscow

ADD ${S6_OVERLAY_RELEASE} /tmp/s6overlay.tar.gz

RUN set -eux \
    && apk upgrade --update --no-cache \
    && apk add --no-cache --virtual .tz \
        alpine-conf \
        tzdata \
    && /sbin/setup-timezone -z $TZ \
    && apk del .tz \
    && rm -rf /var/cache/apk/* \
    && tar xzf /tmp/s6overlay.tar.gz -C / \
    && rm /tmp/s6overlay.tar.gz

RUN set -ex \
    && addgroup -g 82 -S www-data \
    && adduser -u 82 -D -S -G www-data www-data

RUN set -ex \
    && echo -e "http://mirror.yandex.ru/mirrors/alpine/v3.13/main/\nhttp://mirror.yandex.ru/mirrors/alpine/v3.13/community/" > /etc/apk/repositories \
    && apk add --no-cache \
        php7 \
        php7-fpm \
        php7-mysqli \
        php7-ctype \
        php7-fileinfo \
        php7-json \
        php7-mbstring \
        php7-iconv \
        php7-zlib \
        php7-curl \
        php7-gd \
        php7-simplexml \
        php7-xml \
        php7-ldap \
        php7-imap \
        php7-opcache \
        php7-pecl-apcu \
        php7-xmlrpc \
        php7-exif \
        php7-openssl \
        php7-session \
        php7-intl \
        php7-soap \
        php7-calendar \
        php7-dom \
        php7-gettext \
        php7-posix \
        php7-sodium \
        php7-tokenizer \
        php7-xmlreader \
        php7-xmlwriter \
        php7-bz2 \
        php7-zip

RUN  set -ex \
    && echo -e "TLS_REQCERT allow" > /etc/openldap/ldap.conf

RUN  set -ex \
    && mkdir -p /etc/periodic/1min \
    && echo "*       *       *       *       *       run-parts /etc/periodic/1min" >> /etc/crontabs/root

ADD rootfs /

WORKDIR /app

EXPOSE 9000

ENTRYPOINT ["/init"]
