FROM ghcr.io/eklipse2k8/magick-build:bullseye-slim

LABEL org.opencontainers.image.source https://github.com/eklipse2k8/ImageMagickDocker

RUN apt-get update \
    && apt-get install -qq -y curl libjpeg-dev libpng-dev libtiff-dev libgif-dev \
    && mkdir -p /build \
    && cd /build \
    && curl -fsSL --http2 https://storage.googleapis.com/downloads.webmproject.org/releases/webp/libwebp-1.3.0.tar.gz | tar xz \
    && cd libwebp-1.3.0 \
    && ./configure --disable-static --prefix=${MAGICK_PATH} \
    && make \
    && make install \
    && cd / \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -qq -y --auto-remove curl libjpeg-dev libpng-dev libtiff-dev libgif-dev \
    && apt-get clean -qq -y \
    && rm -rf /var/lib/apt/lists/* \
    && webpinfo -version
