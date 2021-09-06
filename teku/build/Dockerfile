FROM consensys/teku:latest

USER root
RUN mkdir /teku && chown teku:teku /teku
USER teku

ENV TEKU_DATA_PATH /teku

ENTRYPOINT /opt/teku/bin/teku $EXTRA_OPTIONS

