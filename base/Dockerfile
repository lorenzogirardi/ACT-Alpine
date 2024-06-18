FROM alpine:3.20

LABEL author="k8s.it <l@k8s.it>"
LABEL layer=base
LABEL area=platform

RUN apk update && \
    apk upgrade && \
    apk add --no-cache ca-certificates lsof procps net-tools file wget && \
    rm -rf /var/cache/apk/* && \
    update-ca-certificates
