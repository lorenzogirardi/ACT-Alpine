# Stage di compilazione per Python dai sorgenti
FROM lgirardi/lg-baseimg-alpine:edge AS builder
LABEL author="k8s.it <l@k8s.it>"
LABEL stage=build

RUN (apt-get update && apt-get install -y \
    build-essential \
    ca-certificates \
    curl \
    libbz2-dev \
    libffi-dev \
    libgdbm-dev \
    libgdbm-compat-dev \
    liblzma-dev \
    libncurses5-dev \
    libreadline6-dev \
    libsqlite3-dev \
    libssl-dev \
    lzma-dev \
    tk-dev \
    uuid-dev \
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*) || true

RUN (apk add --no-cache \
		gnupg \
		tar \
		xz \
		curl \
		bluez-dev \
		bzip2-dev \
		dpkg-dev dpkg \
		findutils \
		gcc \
		gdbm-dev \
		libc-dev \
		libffi-dev \
		libnsl-dev \
		libtirpc-dev \
		linux-headers \
		make \
		ncurses-dev \
		openssl-dev \
		pax-utils \
		readline-dev \
		sqlite-dev \
		tcl-dev \
		tk \
		tk-dev \
		util-linux-dev \
		zlib) || true



# Definire la versione di Python da installare
ARG PYTHON_VERSION=3.12.2

# Scaricare e compilare Python dai sorgenti
WORKDIR /usr/src
RUN curl -O https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz && \
    tar -xzf Python-${PYTHON_VERSION}.tgz && \
    cd Python-${PYTHON_VERSION} && \
    ./configure --enable-optimizations --prefix=/usr/local/python && \
    make -j $(nproc) && \
    make install && \
    cd .. && \
    rm -rf Python-${PYTHON_VERSION}*

# Preparare il pacchetto pip
RUN /usr/local/python/bin/python3 -m ensurepip && \
    /usr/local/python/bin/pip3 install --no-cache --upgrade pip setuptools wheel

# Immagine finale
FROM lgirardi/lg-baseimg-alpine:edge
LABEL author="k8s.it <l@k8s.it>"
LABEL layer=python
LABEL area=platform

# Installare solo le librerie di runtime necessarie
RUN (apk update && \
    apk add --no-cache \
    libffi \
    readline \
    sqlite-libs \
    openssl \
    bzip2 \
    xz \
    zlib) || true

RUN (apt-get update && apt-get install -y \
    libffi8 \
    libreadline8 \
    libsqlite3-0 \
    libssl3 \
    libbz2-1.0 \
    zlib1g \
    && rm -rf /var/lib/apt/lists/*) || true

# Copiare solo i file binari di Python dall'immagine di build
COPY --from=builder /usr/local/python /usr/local/python

# Creare i link simbolici in /usr/bin
RUN (ln -sf /usr/local/python/bin/python3 /usr/bin/python3 && \
    ln -sf /usr/local/python/bin/python3 /usr/bin/python && \
    ln -sf /usr/local/python/bin/pip3 /usr/bin/pip3 && \
    ln -sf /usr/local/python/bin/pip3 /usr/bin/pip ) || true

RUN find / -name python3

RUN (ln -sf /usr/local/python/bin/python3 /usr/bin/python3 && \
    ln -sf /usr/local/python/bin/python3 /usr/bin/python && \
    ln -sf /usr/local/python/bin/pip3 /usr/bin/pip3 && \
    ln -sf /usr/local/python/bin/pip3 /usr/bin/pip) || true

# Optionally add /usr/local/python/bin to PATH, if not already
ENV PATH="/usr/local/python/bin:${PATH}"


# Imposta variabili d'ambiente
ENV PYTHONUNBUFFERED=1
ENV PYTHONPATH=/usr/local/python/lib/python3.12/site-packages
ENV PATH="/usr/local/python/bin:${PATH}"

# Rimuovere il file EXTERNALLY-MANAGED se esiste
RUN find /usr/local/python/lib -name "EXTERNALLY-MANAGED" -delete

# Verifica l'installazione
RUN python --version && pip --version

