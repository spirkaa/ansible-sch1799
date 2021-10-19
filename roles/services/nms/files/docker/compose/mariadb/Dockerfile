FROM yobasystems/alpine-mariadb:10.5

ENV TZ=Europe/Moscow

RUN set -eux \
    && echo -e "http://mirror.yandex.ru/mirrors/alpine/v3.13/main/\nhttp://mirror.yandex.ru/mirrors/alpine/v3.13/community/" > /etc/apk/repositories \
    && apk add --no-cache \
        alpine-conf \
        tzdata \
    && /sbin/setup-timezone -z $TZ
