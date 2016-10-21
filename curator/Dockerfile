FROM alpine:3.4

MAINTAINER Pavel Bezrukov, https://bezr.pro

# Alias, DNS or IP of Elasticsearch host to be queried by Elastalert. Set in default Elasticsearch configuration file.
ENV ELASTICSEARCH_HOST elk
# Port on above Elasticsearch host. Set in default Elasticsearch configuration file.
ENV ELASTICSEARCH_PORT 9200
# The cleaning period of history logs
ENV CURATOR_CLEAR_DAYS 14

RUN apk --update add python py-pip && \
    pip install elasticsearch-curator==3.5

# add tasks scripts
COPY ./tasks /etc/periodic/daily
RUN chmod a+x /etc/periodic/daily/*

CMD ["crond", "-f", "-l", "8"]
