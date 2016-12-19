FROM sebp/elk

MAINTAINER Pavel Bezrukov, https://bezr.pro

ENV ES_HOME /usr/share/elasticsearch
ENV LOGSTASH_FILTER_POSTFIX_TIMESTAMP_TIMEZONE Europe/Moscow

ARG ES_PLUGIN_WATCHER_INSTALL=false
ARG LOGSTASH_KEYS_DIR=./keys

# Installing Elasticsearch plugins
WORKDIR ${ES_HOME}
RUN \
    # The Elastic HQ interface will be accessible at http://<your-host>:9200/_plugin/hq/ (e.g. http://localhost:9200/_plugin/hq/ for a local native instance of Docker).
    gosu elasticsearch bin/plugin install royrusso/elasticsearch-HQ && \
    # To verify that Watcher is set up, call the Watcher _stats API:
    # curl -XGET 'http://localhost:9200/_watcher/stats?pretty'
    # It requires a commercial license.
    if ${ES_PLUGIN_WATCHER_INSTALL}; then \
        gosu elasticsearch bin/plugin install license; \
        gosu elasticsearch bin/plugin install watcher; \
    fi

# Installing Kibana plugins
WORKDIR ${KIBANA_HOME}
RUN gosu kibana bin/kibana plugin -i elastic/sense

# Installing Logstash plugins
WORKDIR ${LOGSTASH_HOME}
RUN gosu logstash bin/logstash-plugin install logstash-filter-environment

# Download Latest GeoIP Database
RUN mkdir -p /usr/share/GeoIP/ && cd /usr/share/GeoIP/ && \
    curl -O "http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz" && \
    gunzip GeoLiteCity.dat.gz && chmod -R +r .

# certs/keys for Beats and Lumberjack input
COPY ${LOGSTASH_KEYS_DIR}/logstash-forwarder.crt ${LOGSTASH_KEYS_DIR}/logstash-beats.crt /etc/pki/tls/certs/
COPY ${LOGSTASH_KEYS_DIR}/logstash-forwarder.key ${LOGSTASH_KEYS_DIR}/logstash-beats.key /etc/pki/tls/private/

# filters
COPY ./filters/ /etc/logstash/conf.d/

# patterns
COPY ./patterns/ ${LOGSTASH_HOME}/patterns/
