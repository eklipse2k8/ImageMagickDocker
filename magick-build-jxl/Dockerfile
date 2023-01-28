FROM jarjoura/magick-build:buster-slim

ENV LIBJXL_VERSION=0.8.0 

ARG TARGETARCH

COPY --from=jarjoura/magick-build-avif:buster-slim /opt/magick /opt/magick
COPY --from=jarjoura/magick-build-webp:buster-slim /opt/magick /opt/magick

ENV LD_LIBRARY_PATH="/opt/magick/lib:/opt/magick/lib/aarch64-linux-gnu:/opt/magick/lib/x86_64-linux-gnu"

RUN apt-get update \
    && apt-get install -y git libjpeg-dev libpng-dev libtiff-dev libgif-dev \
    && mkdir -p /build \
    && cd /build \
    && git clone -b v${LIBJXL_VERSION} https://github.com/libjxl/libjxl.git --depth 1 --recursive --shallow-submodules \
    && ldconfig \
    && cd libjxl; mkdir build; cd build \
    && cmake -DCMAKE_PREFIX_PATH=${MAGICK_PATH} -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTING=OFF .. \
    && cmake --build . -- -j$(nproc) \
    && cmake --install . --prefix ${MAGICK_PATH} \
    && cd / \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -y --auto-remove git libjpeg-dev libpng-dev libtiff-dev libgif-dev \
    && rm -rf /var/lib/apt/lists/* 
