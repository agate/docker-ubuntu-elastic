# This Dockerfile was generated from templates/Dockerfile.j2

FROM ubuntu:16.04
LABEL maintainer "Hao Hong <agate.hao@gmail.com>"

ENV ELASTIC_CONTAINER true
ENV PATH /usr/share/elasticsearch/bin:$PATH
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/jre

RUN apt-get update -y && apt-get install -y openjdk-8-jdk-headless curl wget && apt-get clean

RUN groupadd --gid 1000 elasticsearch && adduser --uid 1000 --gid 1000 --home /usr/share/elasticsearch --disabled-password --gecos ''  elasticsearch

WORKDIR /usr/share/elasticsearch

# Download and extract defined ES version. busybox tar can't strip leading dir.
RUN wget --progress=bar:force https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.5.0.tar.gz && \
    tar zxf elasticsearch-5.5.0.tar.gz && \
    chown -R elasticsearch:elasticsearch elasticsearch-5.5.0 && \
    mv elasticsearch-5.5.0/* . && \
    rmdir elasticsearch-5.5.0 && \
    rm elasticsearch-5.5.0.tar.gz

RUN set -ex && for esdirs in config data logs; do \
        mkdir -p "$esdirs"; \
        chown -R elasticsearch:elasticsearch "$esdirs"; \
    done

USER elasticsearch

# Install and also the ingest-{agent,geoip} modules required for Filebeat
RUN for PLUGIN in ingest-user-agent ingest-geoip; do \
      elasticsearch-plugin install --batch "$PLUGIN"; \
    done
COPY elasticsearch.yml log4j2.properties config/
COPY bin/es-docker bin/es-docker

USER root
RUN chown elasticsearch:elasticsearch \
      config/elasticsearch.yml \
      config/log4j2.properties \
      bin/es-docker && \
    chmod 0750 bin/es-docker

USER elasticsearch
CMD ["/bin/bash", "bin/es-docker"]

EXPOSE 9200 9300
