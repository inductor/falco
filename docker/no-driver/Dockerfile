FROM ubuntu:18.04 as ubuntu

LABEL maintainer="cncf-falco-dev@lists.cncf.io"

ARG FALCO_VERSION
ARG VERSION_BUCKET=bin

ENV FALCO_VERSION=${FALCO_VERSION}
ENV VERSION_BUCKET=${VERSION_BUCKET}

WORKDIR /

ADD https://bintray.com/api/ui/download/falcosecurity/${VERSION_BUCKET}/x86_64/falco-${FALCO_VERSION}-x86_64.tar.gz /

RUN tar -xvf falco-${FALCO_VERSION}-x86_64.tar.gz && \
    rm -f falco-${FALCO_VERSION}-x86_64.tar.gz && \
    mv falco-${FALCO_VERSION}-x86_64 falco && \
    rm -rf falco/usr/src/falco-* falco/usr/bin/falco-driver-loader

RUN sed -e 's/time_format_iso_8601: false/time_format_iso_8601: true/' < /falco/etc/falco/falco.yaml > /falco/etc/falco/falco.yaml.new \
    && mv /falco/etc/falco/falco.yaml.new /falco/etc/falco/falco.yaml

FROM scratch

COPY --from=ubuntu /falco /

CMD ["/usr/bin/falco", "-o", "time_format_iso_8601=true"]