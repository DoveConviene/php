FROM doveconviene/php:7.1.20-apache-api

# image processing libs for imageworker
RUN apt-get update && apt-get install -y imagemagick ghostscript
RUN apt-get update && apt-get install -y poppler-utils
RUN apt-get update && apt-get install -y nip2 && apt-get install -y libvips-tools

# gdal binaries for shapeworkers
RUN apt-get update && apt-get install -y gdal-bin

# Install mozjpeg
RUN cd /tmp/ && \
    apt-get -y install autoconf automake libtool nasm make pkg-config && \
    curl -L -O https://github.com/mozilla/mozjpeg/releases/download/v3.1/mozjpeg-3.1-release-source.tar.gz && \
    tar zxvf mozjpeg-3.1-release-source.tar.gz && \
    cd mozjpeg && \
    autoreconf -fiv && \
    ./configure --enable-shared --prefix=/usr/local LDFLAGS="-Wl,--rpath=/usr/local/lib" && \
    make && \
    make install && \
    ln -s /usr/local/bin/jpegtran /usr/local/bin/mozjpegtran

