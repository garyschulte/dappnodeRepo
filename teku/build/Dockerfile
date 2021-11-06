FROM consensys/teku:21.10.1-jdk17

USER root
RUN mkdir /teku && chown teku:teku /teku
USER teku
EXPOSE 8008

ENV TEKU_DATA_PATH /teku
ENV TEKU_METRICS_ENABLED true
ENV TEKU_METRICS_HOST_ALLOWLIST *

ENTRYPOINT /opt/teku/bin/teku $EXTRA_OPTIONS

