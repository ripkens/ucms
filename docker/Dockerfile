FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        clamav \
        clamav-daemon \
        freshclam \
        chkrootkit \
        rkhunter \
        yara \
        unzip \
        unar \
        unrar-free \
        p7zip-full \
        p7zip \
        bzip2 \
        gzip \
        xz-utils \
        tar \
        libarchive-tools \
        file \
        wget \
        curl \
        ca-certificates \
        && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

RUN freshclam --no-warnings

WORKDIR /scan
