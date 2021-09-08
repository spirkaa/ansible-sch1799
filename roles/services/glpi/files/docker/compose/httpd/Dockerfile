FROM alpine:3.13
LABEL maintainer="Ilya Pavlov <piv@devmem.ru>"

ENV TZ=Europe/Moscow

RUN set -ex \
    && addgroup -g 82 -S www-data \
    && adduser -u 82 -D -S -G www-data www-data

RUN set -eux \
    && echo -e "@testing http://mirror.yandex.ru/mirrors/alpine/edge/testing\nhttp://mirror.yandex.ru/mirrors/alpine/v3.13/main/\nhttp://mirror.yandex.ru/mirrors/alpine/v3.13/community/" > /etc/apk/repositories \
    && apk add --no-cache \
        apache2 \
        apache2-proxy \
        apache-mod-auth-gssapi@testing \
    && apk add --no-cache --virtual .tz \
        alpine-conf \
        tzdata \
    && /sbin/setup-timezone -z $TZ \
    && apk del .tz \
    && mkdir -p /etc/krb5.conf.d /var/lib/sss/pubconf/krb5.include.d

ADD rootfs /

WORKDIR /app

EXPOSE 80

CMD ["httpd", "-D", "FOREGROUND"]
