# This Dockerfile was generated from templates/Dockerfile.j2
FROM ubuntu:16.04
LABEL maintainer "Hao Hong <agate.hao@gmail.com>"

# Install Java and the "which" command, which is needed by Logstash's shell
# scripts.
RUN apt-get update -y && apt-get install -y openjdk-8-jdk-headless curl wget && apt-get clean

# Provide a non-root user to run the process.
RUN groupadd --gid 1000 logstash && adduser --uid 1000 --gid 1000 --home /usr/share/logstash --disabled-password --gecos '' --no-create-home logstash

# Add Logstash itself.
RUN curl -Lo - https://artifacts.elastic.co/downloads/logstash/logstash-5.5.0.tar.gz | \
    tar zxf - -C /usr/share && \
    mv /usr/share/logstash-5.5.0 /usr/share/logstash && \
    chown --recursive logstash:logstash /usr/share/logstash/ && \
    ln -s /usr/share/logstash /opt/logstash

ENV ELASTIC_CONTAINER true
ENV PATH=/usr/share/logstash/bin:$PATH

# Provide a minimal configuration, so that simple invocations will provide
# a good experience.
ADD config/logstash.yml config/log4j2.properties /usr/share/logstash/config/
ADD pipeline/default.conf /usr/share/logstash/pipeline/logstash.conf
RUN chown --recursive logstash:logstash /usr/share/logstash/config/ /usr/share/logstash/pipeline/

# Ensure Logstash gets a UTF-8 locale by default.
ENV LANG='en_US.UTF-8' LC_ALL='en_US.UTF-8'

# Place the startup wrapper script.
ADD bin/docker-entrypoint /usr/local/bin/
RUN chmod 0755 /usr/local/bin/docker-entrypoint

USER logstash


ADD env2yaml/env2yaml /usr/local/bin/

EXPOSE 9600 5044

ENTRYPOINT ["/usr/local/bin/docker-entrypoint"]
