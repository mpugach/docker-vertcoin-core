FROM debian:stable-slim

LABEL maintainer="Maksym Pugach <pugach.m@gmail.com>"

RUN mkdir -p /vertcoin

WORKDIR /vertcoin

ENV GOSU_VERSION=1.10 \
    VERTCOIN_VERSION=0.13.2

RUN useradd -r vertcoin \
 && apt-get update -y \
 && apt-get install -y curl gnupg unzip \
 && for key in \
      B42F6819007F00F88E364FD4036A9C25BF357DD4 \
      425776E2F9E5BAB8 \
    ; do \
      gpg --keyserver pgp.mit.edu --recv-keys "$key" || \
      gpg --keyserver keyserver.pgp.com --recv-keys "$key" || \
      gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key" || \
      gpg --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" ; \
    done \
 && curl -OfSL https://github.com/vertcoin-project/vertcoin-core/releases/download/${VERTCOIN_VERSION}/vertcoind-v${VERTCOIN_VERSION}-linux-amd64.zip \
 && curl -OfSL https://github.com/vertcoin-project/vertcoin-core/releases/download/${VERTCOIN_VERSION}/vertcoind-v${VERTCOIN_VERSION}-linux-amd64.zip.sig \
 && gpg --verify vertcoind-v${VERTCOIN_VERSION}-linux-amd64.zip.sig \
 && unzip vertcoind-v${VERTCOIN_VERSION}-linux-amd64.zip \
 && rm -rf share/ vertcoind-v${VERTCOIN_VERSION}-linux-amd64.zip* \
 && curl -o /usr/local/bin/gosu -fSL https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-$(dpkg --print-architecture) \
 && curl -o /usr/local/bin/gosu.asc -fSL https://github.com/tianon/gosu/releases/download/${GOSU_VERSION}/gosu-$(dpkg --print-architecture).asc \
 && gpg --verify /usr/local/bin/gosu.asc \
 && rm /usr/local/bin/gosu.asc \
 && apt-get purge gnupg unzip -y \
 && apt-get autoremove -y \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
 && chmod +x /usr/local/bin/gosu

ENV VERTCOIN_DATA="/home/vertcoin/.vertcoin" PATH="/vertcoin:$PATH"

COPY docker-entrypoint.sh /entrypoint.sh

RUN chmod u+x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]

CMD ["vertcoind"]
