FROM debian:stable-slim

LABEL maintainer="Maksym Pugach <pugach.m@gmail.com>"

RUN mkdir -p /vertcoin

WORKDIR /vertcoin

RUN apt-get update -y \
 && apt-get install -y curl unzip \
 && curl -OL https://github.com/vertcoin-project/vertcoin-core/releases/download/0.13.2/vertcoind-v0.13.2-linux-amd64.zip \
 && echo 'ad106e31119d1593b6046edd5c51303dd43c6fb2b60bd90616a46106731b122b  vertcoind-v0.13.2-linux-amd64.zip' | sha256sum -c \
 && unzip vertcoind-v0.13.2-linux-amd64.zip \
 && rm -rf vertcoind-v0.13.2-linux-amd64.zip \
 && apt-get purge -y unzip \
 && apt-get clean

ENV VERTCOIN_DATA="/home/vertcoin/.vertcoin" PATH="/vertcoin:$PATH"

COPY docker-entrypoint.sh /entrypoint.sh

RUN chmod u+x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]

CMD ["vertcoind"]
