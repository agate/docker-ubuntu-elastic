# This Dockerfile was generated from templates/Dockerfile.j2
FROM ubuntu:16.04
LABEL maintainer "Hao Hong <agate.hao@gmail.com>"
EXPOSE 5601

# Add Reporting dependencies.
RUN apt-get update -y && apt-get install -y openjdk-8-jdk-headless curl wget && apt-get clean

WORKDIR /usr/share/kibana
RUN curl -Ls https://artifacts.elastic.co/downloads/kibana/kibana-5.5.0-linux-x86_64.tar.gz | tar --strip-components=1 -zxf - && \
    ln -s /usr/share/kibana /opt/kibana

ENV ELASTIC_CONTAINER true
ENV PATH=/usr/share/kibana/bin:$PATH

# Set some Kibana configuration defaults.
ADD config/kibana.yml /usr/share/kibana/config/

# Add the launcher/wrapper script. It knows how to interpret environment
# variables and translate them to Kibana CLI options.
ADD bin/kibana-docker /usr/local/bin/

# Add a self-signed SSL certificate for use in examples.
ADD ssl/kibana.example.org.* /usr/share/kibana/config/

# Provide a non-root user to run the process.
RUN groupadd --gid 1000 kibana && adduser --uid 1000 --gid 1000 --home /usr/share/kibana --disabled-password --gecos '' --no-create-home kibana

USER kibana

CMD /usr/local/bin/kibana-docker
