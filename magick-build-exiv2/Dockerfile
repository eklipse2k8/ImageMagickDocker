FROM ghcr.io/eklipse2k8/magick-build-jxl:bullseye-slim

LABEL org.opencontainers.image.source https://github.com/eklipse2k8/ImageMagickDocker

RUN apt-get update \
    && apt-get install -y curl libjpeg62-turbo-dev libjpeg-dev libpng-dev libexpat1-dev \
    && mkdir -p /build \
    && cd /build \
    && curl -fsSL --http2 https://github.com/Exiv2/exiv2/archive/refs/tags/v0.27.6.tar.gz | tar xz \
    && cd exiv2-0.27.6 \
    && mkdir build \
    && cd build \
    && cmake -DCMAKE_INSTALL_PREFIX=${MAGICK_PATH} -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=ON \
      -DEXIV2_ENABLE_BROTLI=ON .. \
    && cmake --build . -- -j$(nproc) \
    && cmake --install . --prefix ${MAGICK_PATH} \
    && cd / \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -y --auto-remove curl libjpeg62-turbo-dev libjpeg-dev libpng-dev libexpat1-dev \
    && rm -rf /var/lib/apt/lists/*
