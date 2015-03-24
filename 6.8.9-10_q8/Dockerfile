# Start with Debian Wheezy
FROM debian:wheezy

MAINTAINER Jamin Kortegard <jamink919@gmail.com>

# Get basic utils
RUN apt-get update && apt-get install -y --no-install-recommends \
        apt-utils \
        bzip2 \
        ca-certificates \
        curl \
        wget \
    && rm -rf /var/lib/apt/lists/*

# Install dependencies for ImageMagick.
RUN apt-get -y update && apt-get install -y --no-install-recommends \
        dcraw \
        libfreetype6 \
        libfreetype6-dev \
        libgif4 \
        libgif-dev \
        libjasper1 \
        libjasper-dev \
        libjpeg8 \
        libjpeg8-dev \
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

# Compilers for Ghostscript and ImageMagick.
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

# Install ImageMagick with PerlMagick module
ENV MAGICK_VER 6.8.9-10

RUN apt-get -y update && apt-get install -y --no-install-recommends \
    libperl-dev \
    && curl -L -O http://www.imagemagick.org/download/releases/ImageMagick-${MAGICK_VER}.tar.bz2 \
    && tar -xjf ImageMagick-${MAGICK_VER}.tar.bz2 \
    && cd ImageMagick-${MAGICK_VER} \
    && ./configure --enable-shared --with-gslib --with-wmf --without-x --with-xml \
        --with-freetype --with-fontconfig --with-quantum-depth=8 --with-perl=/usr/bin/perl \
    && make \
    && make install \
    && make clean \
    && ldconfig \
    && cd /tmp/install \
    && rm -rf ImageMagick-${MAGICK_VER}* \
    && apt-get -y purge libperl-dev \
    # && apt-get -y purge libperl5.14 \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /

RUN rm -rf /tmp/install

CMD ["convert", "--version"]
