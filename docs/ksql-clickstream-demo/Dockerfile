# https://confluentinc.atlassian.net/browse/KSQL-292

ARG DOCKER_REGISTRY

FROM ${DOCKER_REGISTRY}confluentinc/docker-demo-base:3.3.1

ARG KSQL_VERSION
ARG ARTIFACT_ID

EXPOSE 3000

ENV ES_JAVA_OPTS="-Xms512M -Xmx512M"
ENV KSQL_CLASSPATH=/usr/share/java/${ARTIFACT_ID}/${ARTIFACT_ID}-${KSQL_VERSION}-standalone.jar
ENV KSQL_CONFIG_DIR="/etc/ksql"
ENV KSQL_LOG4J_OPTS="-Dlog4j.configuration=file:/etc/ksql/log4j-rolling.properties"

RUN wget -q https://s3-us-west-2.amazonaws.com/grafana-releases/release/grafana_4.4.3_amd64.deb \
    && wget -q https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.5.2.deb \
    && dpkg -i grafana_4.4.3_amd64.deb \
    && dpkg -i elasticsearch-5.5.2.deb \
    && rm grafana_4.4.3_amd64.deb \
    && rm elasticsearch-5.5.2.deb

ADD target/${ARTIFACT_ID}-${KSQL_VERSION}-standalone.jar /usr/share/java/${ARTIFACT_ID}/${ARTIFACT_ID}-${KSQL_VERSION}-standalone.jar
ADD target/${ARTIFACT_ID}-${KSQL_VERSION}-package/bin/* /usr/bin/
ADD target/${ARTIFACT_ID}-${KSQL_VERSION}-package/etc/* /etc/ksql/

ADD demo/*sh /usr/share/doc/ksql-clickstream-demo/
ADD demo/*sql /usr/share/doc/ksql-clickstream-demo/
ADD demo/*json /usr/share/doc/ksql-clickstream-demo/
ADD demo/connect-config/null-filter-4.0.0-SNAPSHOT.jar /usr/share/java/kafka-connect-elasticsearch/
