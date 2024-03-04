FROM lgirardi/lg-baseimg-alpine:edge

LABEL author="k8s.it <l@k8s.it>"
LABEL layer=flask
LABEL area=platform

ENV PYTHONUNBUFFERED=1

RUN echo "**** install Python ****" && \
    apk add --no-cache python3=3.11.8-r0 && \
    if [ ! -e /usr/bin/python ]; then ln -sf python3 /usr/bin/python ; fi && \
    \
    echo "**** install pip ****" && \
    rm /usr/lib/python3.11*/EXTERNALLY-MANAGED && \
    python -m ensurepip && \
    rm -r /usr/lib/python*/ensurepip && \
    if [ ! -e /usr/bin/pip ]; then ln -s pip3 /usr/bin/pip ; fi && \
    pip install --no-cache --upgrade pip setuptools wheel



