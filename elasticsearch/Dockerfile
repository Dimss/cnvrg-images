FROM registry.access.redhat.com/ubi8/ubi:latest
ARG ES_URL=https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.2.4.tar.gz
ARG HOME=/usr/share/elasticsearch
ARG USER=es
ARG USER_ID=1002
WORKDIR $HOME
LABEL name="cnvrg-elasticsearch" \
          vendor="cnvrg.io" \
          version="7.4.0" \
          release="7.4.0" \
          summary="ElasticSearch for cnvrg.io platform" \
          description="ElasticSearch for cnvrg.io platform"
RUN useradd -rm -d $HOME -s /bin/bash -g root -u $USER_ID $USER
RUN curl $ES_URL --output es.tar.gz && \
        tar zxvf es.tar.gz && \
        rm -f es.tar.gz && \
        mv elasticsearch* elasticsearch
COPY license /license
RUN chown -R es:0 $HOME && \
    chgrp -R 0 $HOME && \
    chmod -R g=u $HOME
USER es
CMD "/usr/share/elasticsearch/bin/elasticsearch"