FROM sebp/elk

ENV ES_HOME /usr/share/elasticsearch

# Installing Elasticsearch plugins
WORKDIR ${ES_HOME}
RUN gosu elasticsearch bin/plugin install license &&
    # The Elastic HQ interface will be accessible at http://<your-host>:9200/_plugin/hq/ (e.g. http://localhost:9200/_plugin/hq/ for a local native instance of Docker).
    gosu elasticsearch bin/plugin install royrusso/elasticsearch-HQ &&
    # To verify that Watcher is set up, call the Watcher _stats API:
    # curl -XGET 'http://localhost:9200/_watcher/stats?pretty'
    bin/plugin install watcher

# Installing Kibana plugins
WORKDIR ${KIBANA_HOME}
RUN gosu kibana bin/kibana plugin -i elastic/sense

# Download Latest GeoIP Database
RUN mkdir -p /usr/share/GeoIP/ && cd /usr/share/GeoIP/ &&
    curl -O "http://geolite.maxmind.com/download/geoip/database/GeoLiteCity.dat.gz" &&
    gunzip GeoLiteCity.dat.gz && chmod -R +r .

# certs/keys for Beats and Lumberjack input
ADD ./keys/logstash-forwarder.crt ./keys/logstash-beats.crt /etc/pki/tls/certs/
ADD ./keys/logstash-forwarder.key ./keys/logstash-beats.key /etc/pki/tls/private/

# filters
ADD ./filters/ /etc/logstash/conf.d/

# patterns
ADD ./patterns/ ${LOGSTASH_HOME}/patterns/