###
# Builder container
###
FROM golang:buster AS builder

ARG tags=none

RUN go version && \
    apt-get install gcc git curl make gcc g++ python && \
    mkdir /pufferpanel && \
    wget https://github.com/swaggo/swag/releases/download/v1.6.3/swag_1.6.3_Linux_x86_64.tar.gz && \
    tar -zxf swag*.tar.gz && \
    mv swag /go/bin/

WORKDIR /build/pufferd
RUN git clone https://github.com/pufferpanel/pufferd .

WORKDIR /build/pufferd
RUN go build -v -tags $tags -o /pufferpanel/pufferd github.com/pufferpanel/pufferd/v2/cmd

###
# Generate final image
###

FROM ubuntu:bionic
COPY --from=builder /pufferpanel /pufferpanel

RUN apt-get update && apt-get install -y locales && rm -rf /var/lib/apt/lists/* \
    && localedef -i en_US -c -f UTF-8 -A /usr/share/locale/locale.alias en_US.UTF-8
ENV LANG en_US.utf8

EXPOSE 5656 5657
VOLUME /var/lib/pufferd

ENV PUFFERD_CONSOLE_BUFFER=50 \
    PUFFERD_CONSOLE_FORWARD=false \
    PUFFERD_LISTEN_WEB=0.0.0.0:5656 \
    PUFFERD_LISTEN_SOCKET=unix:/var/lib/pufferd/pufferd.sock \
    PUFFERD_LISTEN_WEBCERT=/var/lib/pufferd/web/https.pem \
    PUFFERD_LISTEN_WEBKEY=/var/lib/pufferd/web/https.key \
    PUFFERD_LISTEN_SFTP=0.0.0.0:5657 \
    PUFFERD_LISTEN_SFTPKEY=/var/lib/pufferd/sftp.key \
    PUFFERD_AUTH_PUBLICKEY=/var/lib/pufferd/panel.pem \
    PUFFERD_AUTH_URL=unix:/var/lib/pufferd/pufferpanel.sock \
    PUFFERD_AUTH_CLIENTID=unknown \
    PUFFERD_AUTH_CLIENTSECRET=unknown \
    PUFFERD_DATA_CACHE=/var/lib/pufferd/cache \
    PUFFERD_DATA_SERVERS=/var/lib/pufferd/servers \
    PUFFERD_DATA_MODULES=/var/lib/pufferd/modules \
    PUFFERD_DATA_LOGS=/var/lib/pufferd/logs \
    PUFFERD_DATA_CRASHLIMIT=3

WORKDIR /var/lib/pufferd

ENTRYPOINT ["/pufferpanel/pufferd"]
CMD ["run"]