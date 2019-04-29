FROM debian:stable AS compiler
MAINTAINER Arnaud Fenioux <afenioux _ hopus.net> 
EXPOSE 179

# This Dockerfile is inspired from https://hub.docker.com/r/pierky/bird
# This is a multistage build to reduce disk usage (you may also want to try afenioux/bird:alpine instead of debian)
# thanks to @jptazzo https://intro-2019-04.container.training/#196
# This image is build with the command : `docker build -t afenioux/bird:2.0.4 --build-arg BIRDV=2.0.4 .`

# The host's file at /somewhere/bird/bird.conf is used as the configuration file for BIRD (/etc/bird/bird.conf in the containter)
# if you run with this option : `docker run -d -v /mnt/flash/docker/bird:/etc/bird:rw --memory 512m --memory-swap 512m --cpus 1.1 afenioux/bird:2.0.4`
# logs are available with `docker logs <container_id> is you set `log stderr all;` in bird.conf, but you may write in a file like /etc/bird/bird.logs

RUN apt-get update && apt-get install -y \
    autoconf \
    bison \
    build-essential \
    curl \
    flex \
    libreadline-dev \
    libncurses5-dev \
    m4 \
    unzip

WORKDIR /tmp
ARG BIRDV
RUN curl -O -L ftp://bird.network.cz/pub/bird/bird-${BIRDV}.tar.gz
RUN tar -xvzf bird-${BIRDV}.tar.gz

WORKDIR /tmp/bird-${BIRDV}
RUN ./configure --prefix=/usr --sysconfdir=/etc/bird --localstatedir=/var --with-runtimedir=/run/bird && \
    make

FROM debian:stable
RUN apt-get update && apt-get install -y libreadline7 libncurses5
ARG BIRDV
COPY --from=compiler /tmp/bird-${BIRDV}/bird /tmp/bird-${BIRDV}/birdc /tmp/bird-${BIRDV}/birdcl /usr/local/sbin/
RUN mkdir /etc/bird /run/bird

CMD bird -d
