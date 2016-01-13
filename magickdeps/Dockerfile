# Start with Debian Jessie
FROM debian:jessie

MAINTAINER Jamin Kortegard <jamink919@gmail.com>

# Get basic utils
RUN apt-get update && apt-get install -y --no-install-recommends \
        apt-utils \
        bzip2 \
        ca-certificates \
        curl \
        wget \
        && rm -rf /var/lib/apt/lists/*

# Install dependencies for ImageMagick/GraphicsMagick.
RUN apt-get -y update && apt-get install -y --no-install-recommends \
        dcraw \
        libfreetype6 \
        libfreetype6-dev \
        libgif4 \
        libgif-dev \
        libjasper1 \
        libjasper-dev \
        libturbojpeg1 \
        libturbojpeg1-dev \
        liblcms2-2 \
        liblcms2-dev \
        libpng12-0 \
        libpng12-dev \
        libtiff5 \
        libtiff5-dev \
        libwmf0.2-7 \
        libwmf-dev \
        libxml2 \
        libxml2-dev \
        zlib1g \
        zlib1g-dev \
        && rm -rf /var/lib/apt/lists/*


# Create temp dir for installations
WORKDIR /tmp/install

# Compilers for Ghostscript and GraphicsMagick/ImageMagick.
# This will be wasted layer space afterwards. How to optimize?
RUN apt-get -y update && apt-get install -y --no-install-recommends \
        autoconf \
        automake \
        build-essential \
        libx11-dev \
        && rm -rf /var/lib/apt/lists/*

# Install specific Ghostscript 9.15
ENV GS_VER 9.15

RUN curl -L -O http://downloads.ghostscript.com/public/ghostscript-${GS_VER}.tar.gz \
    && tar -xzf ghostscript-${GS_VER}.tar.gz\
    && cd ghostscript-${GS_VER} \
    && ./configure \
    && make \
    && make install \
    && make clean \
    && ldconfig \
    && cd /tmp/install \
    && rm -rf ghostscript-${GS_VER}*
