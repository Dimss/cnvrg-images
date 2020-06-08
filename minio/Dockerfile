FROM registry.access.redhat.com/ubi8/ubi:latest
LABEL maintainer="MinIO Inc <dev@min.io>"
ENV GOPATH /go
ENV CGO_ENABLED 0
ENV GO111MODULE on
RUN  \
     dnf -y install git golang && \
     git clone -b RELEASE.2019-04-09T01-22-30Z https://github.com/minio/minio
RUN  \
     cd minio && \
     go install -v -ldflags "$(go run buildscripts/gen-ldflags.go)" && \
     cd dockerscripts; go build -ldflags "-s -w" -o /usr/bin/healthcheck healthcheck.go
FROM registry.access.redhat.com/ubi8/ubi:latest
ENV MINIO_UPDATE off
ENV MINIO_ACCESS_KEY_FILE=access_key \
    MINIO_SECRET_KEY_FILE=secret_key
EXPOSE 9000
COPY --from=0 /go/bin/minio /usr/bin/minio
COPY --from=0 /usr/bin/healthcheck /usr/bin/healthcheck
COPY dockerscripts/docker-entrypoint.sh /usr/bin/
RUN  \
     # apk add --no-cache ca-certificates 'curl>7.61.0' 'su-exec>=0.2' && \
     echo 'hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4' >> /etc/nsswitch.conf
ENTRYPOINT ["/usr/bin/docker-entrypoint.sh"]
VOLUME ["/data"]
HEALTHCHECK --interval=1m CMD healthcheck
CMD ["minio"]
