FROM lgirardi/lg-baseimg-alpine:edge

LABEL author="k8s.it <l@k8s.it>"
LABEL layer=nodejs
LABEL area=platform


RUN echo "**** install nodejs ****"  \
    && apk update && apk add --no-cache wget \
    && apk add --no-cache libuv \
    && apk add --no-cache --update-cache  nodejs=20.12.1-r0 npm \
    && echo "NodeJS Version:" "$(node -v)" \
    && echo "NPM Version:" "$(npm -v)"


