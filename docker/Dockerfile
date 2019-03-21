FROM jenkins/jnlp-slave:alpine

USER root

ARG GLIBC=2.28-r0
ARG DOCKER_VERSION=18.06.1
ARG DOCKER_COMPOSE_VERSION=1.23.2

# docker-compose is built against glibc and the compat package isn't quite enough
# steps from https://github.com/docker/compose/blob/1.23.2/Dockerfile.run
RUN apk update && apk add --no-cache openssl ca-certificates curl libgcc && \
    curl -fsSL -o /etc/apk/keys/sgerrand.rsa.pub https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub && \
    curl -fsSL -o glibc-$GLIBC.apk https://github.com/sgerrand/alpine-pkg-glibc/releases/download/$GLIBC/glibc-$GLIBC.apk && \
    apk add --no-cache glibc-$GLIBC.apk && \
    ln -s /lib/libz.so.1 /usr/glibc-compat/lib/ && \
    ln -s /lib/libc.musl-x86_64.so.1 /usr/glibc-compat/lib && \
    ln -s /usr/lib/libgcc_s.so.1 /usr/glibc-compat/lib && \
    rm /etc/apk/keys/sgerrand.rsa.pub glibc-$GLIBC.apk && \
    apk del curl

# can copy binaries from the official images
COPY --from=library/docker:${DOCKER_VERSION} /usr/local/bin/docker /usr/local/bin/docker
COPY --from=docker/compose:${DOCKER_COMPOSE_VERSION} /usr/local/bin/docker-compose /usr/local/bin/docker-compose

RUN touch /debug-flag
USER jenkins
