FROM clojure:lein@sha256:8bdf877f3bfaed71b46556ebef62ffbe440f5fe30fe55d30503db37ae8e5a973 AS builder

# ARG MVN_ARTIFACTORYMAVENREPOSITORY_USER
# ARG MVN_ARTIFACTORYMAVENREPOSITORY_PWD

RUN mkdir /build

WORKDIR /build

COPY project.clj /build
COPY src /build/src

RUN lein metajar

FROM gcr.io/personalsdm-216019/java-atomist:0.13.0-20191119164121

MAINTAINER Jim Clark <jim@atomist.com>

# ARG SOURCE
# ARG REVISION
# LABEL org.opencontainers.image.revision=$REVISION
# LABEL org.opencontainers.image.source=$SOURCE

RUN mkdir -p /usr/src/app \
    && mkdir -p /usr/src/app/bin \
    && mkdir -p /usr/src/app/lib

WORKDIR /usr/src/app

COPY --from=builder /build/target/lib /usr/src/app/lib

COPY --from=builder /build/target/metajar/service.jar /usr/src/app/

CMD ["-Djava.net.preferIPv4Stack=true", "-jar", "/usr/src/app/service.jar", "-Dclojure.core.async.pool-size=20"]

ENV APP_NAME=service

EXPOSE 4000
