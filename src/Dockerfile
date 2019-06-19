ARG BASE
FROM ${BASE} AS runtime
ARG ARCH
ARG GAMBIT_VERSION

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update \
 && apt-get dist-upgrade -y \
 && apt-get install -y --no-install-recommends \
        autoconf \
        build-essential \
        git \
        libleveldb-dev \
        libleveldb1v5 \
        liblmdb-dev \
        libmysqlclient-dev \
        libsnappy1v5 \
        libsqlite3-dev \
        libssl-dev \
        libyaml-dev \
        pkg-config \
        rsync \
        texinfo \
        zlib1g-dev \
 && apt-get autoremove -y \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/* /var/tmp/* /tmp/*

RUN git config --global url.https://github.com/.insteadOf git://github.com/ \
 && cd /root \
 && git clone https://github.com/gambit/gambit.git \
 && cd /root/gambit \
 && git fetch -a \
 && git checkout v${GAMBIT_VERSION} \
 && ./configure \
        --prefix=/usr/local/gambit \
        --enable-single-host \
        --enable-openssl \
        --enable-default-runtime-options=f8,-8,t8 \
        --enable-poll \
 && make -j`nproc` \
 && make install \
 && rm -rf /root/gambit

ARG VCS_REF
ARG VCS_URL
ARG BUILD_DATE
LABEL org.label-schema.vcs-ref=${VCS_REF} \
      org.label-schema.vcs-url=${VCS_URL} \
org.label-schema.build-date=${BUILD_DATE}

ENV PATH "$PATH:/usr/local/gambit/bin"
CMD gci
