FROM jarjoura/magick-build-jxl:buster-slim

RUN apt-get update \
    && apt-get install -y curl libde265-dev libjpeg62-turbo-dev libx265-dev libjpeg-dev libpng-dev \
    && mkdir -p /build \
    && cd /build \
    && curl -fsSL --http2 https://github.com/strukturag/libheif/releases/download/v1.14.2/libheif-1.14.2.tar.gz | tar xz \
    && cd libheif-1.14.2 \
    && mkdir build \
    && cd build \
    && cmake -DCMAKE_INSTALL_PREFIX=${MAGICK_PATH} -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=ON \
        -DBUILD_TESTING=OFF .. \
    && cmake --build . -- -j$(nproc) \
    && cmake --install . --prefix ${MAGICK_PATH} \
    && cd / \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -y --auto-remove curl libde265-dev libjpeg62-turbo-dev libx265-dev libjpeg-dev libpng-dev \
    && rm -rf /var/lib/apt/lists/* 
