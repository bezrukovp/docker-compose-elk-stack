FROM sebp/elk

ENV ES_HOME /usr/share/elasticsearch

# certs/keys for Beats and Lumberjack input
ADD ./keys/logstash-forwarder.crt /etc/pki/tls/certs/logstash-forwarder.crt
ADD ./keys/logstash-forwarder.key /etc/pki/tls/private/logstash-forwarder.key
ADD ./keys/logstash-beats.crt /etc/pki/tls/certs/logstash-beats.crt
ADD ./keys/logstash-beats.key /etc/pki/tls/private/logstash-beats.key

# patterns
ADD ./patterns/main.pattern ${LOGSTASH_HOME}/patterns/main

# Installing Elasticsearch plugins
WORKDIR ${ES_HOME}
RUN gosu elasticsearch bin/plugin install license
# The Elastic HQ interface will be accessible at http://<your-host>:9200/_plugin/hq/ (e.g. http://localhost:9200/_plugin/hq/ for a local native instance of Docker).
RUN gosu elasticsearch bin/plugin install royrusso/elasticsearch-HQ
# To verify that Watcher is set up, call the Watcher _stats API:
# curl -XGET 'http://localhost:9200/_watcher/stats?pretty'
RUN bin/plugin install watcher

# Installing Kibana plugins
WORKDIR ${KIBANA_HOME}
RUN gosu kibana bin/kibana plugin -i elastic/sense