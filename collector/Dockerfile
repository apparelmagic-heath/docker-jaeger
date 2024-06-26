ARG DOCKER_TAG levonet/jaeger:latest
FROM ${DOCKER_TAG} AS all-in-one

FROM debian:stretch-slim

COPY --from=all-in-one /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt
COPY --from=all-in-one /opt/bin/jaeger-collector /opt/bin/jaeger-collector
COPY --from=all-in-one /opt/jaeger/plugin/jaeger-clickhouse /opt/jaeger/plugin/jaeger-clickhouse
COPY --from=all-in-one /opt/jaeger/config.yaml /opt/jaeger/config.yaml
COPY --from=all-in-one /opt/jaeger/sqlscripts /opt/jaeger/sqlscripts

ENV SPAN_STORAGE_TYPE grpc-plugin
WORKDIR /opt/jaeger

# Collector OTLP gRPC
EXPOSE 4317

# Collector OTLP HTTP
EXPOSE 4318

EXPOSE 14250
EXPOSE 14268
EXPOSE 14269

ENTRYPOINT ["/opt/bin/jaeger-collector"]
CMD ["--grpc-storage-plugin.binary", "plugin/jaeger-clickhouse", "--grpc-storage-plugin.configuration-file", "config.yaml"]
