FROM debian:bullseye

ENV DANTE_VER 1.4.2
ENV DANTE_URL https://c4s.de/dante-1.4.2.zip
#ENV DANTE_SHA 4c97cff23e5c9b00ca1ec8a95ab22972813921d7fbf60fc453e3e06382fc38a7
ENV DANTE_SHA 43c8289b71dbf596735fb2e74e8bbf2316ce83ae8cd288d0f510397f033ac322
ENV DANTE_FILE dante.tar.gz
ENV DANTE_TEMP dante
ENV DANTE_DEPS build-essential curl openssl unzip

RUN set -xe \
    && apt-get update \
    && apt-get install -y $DANTE_DEPS \
    && mkdir $DANTE_TEMP \
        && cd $DANTE_TEMP \
        && curl -sSL $DANTE_URL -o $DANTE_FILE \
        && echo "$DANTE_SHA *$DANTE_FILE" | shasum -c \
        && unzip $DANTE_FILE \
	&& chmod -R 777 ./ \
	&& cd dante-$DANTE_VER \
        && ./configure \
        && make install \
        && cd .. \
        && rm -rf $DANTE_TEMP \
    && useradd -m -p $(echo verisecurepw | openssl passwd -1 -stdin) -s /bin/bash userX \
    && apt-get purge -y --auto-remove $DANTE_DEPS \
    && rm -rf /var/lib/apt/lists/*

ADD ./sockd.conf /etc/sockd.conf

ENV CFGFILE /etc/sockd.conf
ENV PIDFILE /tmp/sockd.pid
ENV WORKERS 10

EXPOSE 1080

CMD sockd -f $CFGFILE -p $PIDFILE -N $WORKERS
