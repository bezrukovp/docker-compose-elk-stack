FROM ivankrizsan/elastalert

RUN \
    # Set the Elasticsearch host that Elastalert is to query.
    sed -i -e"s|es_host: [[:print:]]*|es_host: ${ELASTICSEARCH_HOST}|g" ${ELASTALERT_CONFIG} && \
    # Set the port used by Elasticsearch at the above address.
    sed -i -e"s|es_port: [0-9]*|es_port: ${ELASTICSEARCH_PORT}|g" ${ELASTALERT_CONFIG}