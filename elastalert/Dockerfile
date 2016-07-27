FROM ivankrizsan/elastalert

MAINTAINER Pavel Bezrukov, https://bezr.pro

ARG setContainerTimezone=false
ARG containerTimezone=Europe/Moscow

RUN \
    if [ "$setContainerTimezone" = "true" ]; then \
        cp /usr/share/zoneinfo/${containerTimezone} /etc/localtime; \
    fi