FROM debian:buster-slim

ADD pump.tar.gz /opt/tyk-pump

VOLUME ["/opt/tyk-pump/conf"]
WORKDIR /opt/tyk-pump

ENTRYPOINT ["/opt/tyk-pump/tyk-pump", "--conf=/opt/tyk-pump/conf/tyk.conf"]
