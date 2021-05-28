FROM clojure:lein@sha256:fae944d1c4a05d3d10eadc51ffaa14c5cd8e15a46ca23a3bb325338f2589f6ac AS builder

ARG MVN_ARTIFACTORYMAVENREPOSITORY_USER
ARG MVN_ARTIFACTORYMAVENREPOSITORY_PWD

RUN mkdir /build

WORKDIR /build

COPY project.clj /build
COPY src /build/src

RUN lein metajar

FROM gcr.io/personalsdm-216019/java-atomist:0.13.0-20191119164121@sha256:60d43b18bb8d86f2906f8a01a51a95dd4a7115ff35a6c921ef1f5b65783d97bd

MAINTAINER Jim Clark <jim@atomist.com>

ARG SOURCE
ARG REVISION
LABEL org.opencontainers.image.revision=$REVISION
LABEL org.opencontainers.image.source=$SOURCE

RUN mkdir -p /usr/src/app \
    && mkdir -p /usr/src/app/bin \
    && mkdir -p /usr/src/app/lib

WORKDIR /usr/src/app

COPY --from=builder /build/target/lib /usr/src/app/lib

COPY --from=builder /build/target/metajar/service.jar /usr/src/app/

CMD ["-Djava.net.preferIPv4Stack=true", "-jar", "/usr/src/app/service.jar", "-Dclojure.core.async.pool-size=20"]

ENV APP_NAME=service

EXPOSE 4000
