# Kafka and Zookeeper

FROM java:8

ENV DEBIAN_FRONTEND noninteractive
ENV SCALA_VERSION 2.11
ENV KAFKA_VERSION 1.0.0
ENV KAFKA_HOME /opt/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION"
ENV TEMP_PATH     /temp/kafka
RUN mkdir -p  ${TEMP_PATH}
COPY sources.list.163 ${TEMP_PATH}/sources.list.163
COPY dazd-maven-3.3.9.tgz /opt/


# Install Kafka, Zookeeper and other needed things
RUN set -x \
    && mv  /etc/apt/sources.list /etc/apt/sources.list.back \
    && cp  "${TEMP_PATH}/sources.list.163" /etc/apt/sources.list \
    && apt-get update --quiet \
    && apt-get install --quiet --yes --no-install-recommends  --allow-unauthenticated  zookeeper wget git  net-tools supervisor dnsutils openssh-server  \
    && apt-get clean \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean && \
    wget -q http://apache.mirrors.spacedump.net/kafka/"$KAFKA_VERSION"/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz -O /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz && \
    tar xfz /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz -C /opt && \
    rm /tmp/kafka_"$SCALA_VERSION"-"$KAFKA_VERSION".tgz

ADD scripts/start-kafka.sh /usr/bin/start-kafka.sh

# Supervisor config
ADD supervisor/kafka.conf supervisor/zookeeper.conf /etc/supervisor/conf.d/

# 2181 is zookeeper, 9092 is kafka
EXPOSE 2181 9092 22

CMD ["supervisord", "-n"]
