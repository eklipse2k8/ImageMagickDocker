FROM ghcr.io/eklipse2k8/magick-build:bullseye-slim AS build-dav1d

RUN apt-get update \
    && apt-get install -qq -y git nasm ninja-build meson \
    && mkdir -p /build \
    && cd /build \
    && git clone -b 1.0.0 --depth 1 https://code.videolan.org/videolan/dav1d.git \
    && cd dav1d; mkdir build; cd build \
    && meson --prefix=${MAGICK_PATH} --buildtype release .. \
    && ninja install \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -qq -y --auto-remove nasm ninja-build meson \
    && apt-get clean -qq -y \
    && rm -rf /var/lib/apt/lists/*

FROM ghcr.io/eklipse2k8/magick-build:bullseye-slim AS build-aom

RUN apt-get update \
    && apt-get install -qq -y git nasm libjpeg-dev \
    && mkdir -p /build \
    && cd /build \
    && git clone -b v3.5.0 --depth 1 https://aomedia.googlesource.com/aom \
    && cd aom; mkdir build.libavif; cd build.libavif \
    && cmake -DCMAKE_INSTALL_PREFIX=${MAGICK_PATH} -DBUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release \
      -DENABLE_DOCS=0 -DENABLE_EXAMPLES=0 -DENABLE_TESTDATA=0 -DENABLE_TESTS=0 -DENABLE_TOOLS=0 .. \
    && cmake --build . -- -j$(nproc) \
    && cmake --install . --prefix ${MAGICK_PATH} \
    && cd / \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -qq -y --auto-remove git nasm libjpeg-dev \
    && apt-get clean -qq -y \
    && rm -rf /var/lib/apt/lists/*

FROM ghcr.io/eklipse2k8/magick-build:bullseye-slim AS build-gav1

RUN apt-get update \
    && apt-get install -qq -y git nasm \
    && mkdir -p /build \
    && cd /build \
    && git clone -b v0.18.0 --depth 1 https://chromium.googlesource.com/codecs/libgav1 \
    && cd libgav1; mkdir build; cd build \
    && cmake -DCMAKE_INSTALL_PREFIX=${MAGICK_PATH} -DBUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release \
      -DLIBGAV1_THREADPOOL_USE_STD_MUTEX=1 -DLIBGAV1_ENABLE_EXAMPLES=0 -DLIBGAV1_ENABLE_TESTS=0 .. \
    && cmake --build . -- -j$(nproc) \
    && cmake --install . --prefix ${MAGICK_PATH} \
    && cd / \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -qq -y --auto-remove git nasm \
    && apt-get clean -qq -y \
    && rm -rf /var/lib/apt/lists/*

FROM ghcr.io/eklipse2k8/magick-build:bullseye-slim AS build-sharpyuv

RUN apt-get update \
    && apt-get install -qq -y git \
    && mkdir -p /build \
    && cd /build \
    && git clone --single-branch https://chromium.googlesource.com/webm/libwebp \
    && cd libwebp; git checkout 15a91ab; mkdir build; cd build \
    && cmake -DCMAKE_INSTALL_PREFIX=${MAGICK_PATH} -DBUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release .. \
    && cmake --build . -- -j$(nproc) \
    && cmake --install . --prefix ${MAGICK_PATH} \
    && cd / \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -qq -y --auto-remove git \
    && apt-get clean -qq -y \
    && rm -rf /var/lib/apt/lists/*

FROM ghcr.io/eklipse2k8/magick-build:bullseye-slim AS build-yuv

RUN apt-get update \
    && apt-get install -qq -y git \
    && mkdir -p /build \
    && cd /build \
    && git clone --single-branch https://chromium.googlesource.com/libyuv/libyuv \
    && cd libyuv; git checkout f9fda6e7; mkdir build; cd build \
    && cmake -DCMAKE_INSTALL_PREFIX=${MAGICK_PATH} -DBUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release .. \
    && cmake --build . -- -j$(nproc) \
    && cmake --install . --prefix ${MAGICK_PATH} \
    && cd / \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -qq -y --auto-remove git \
    && apt-get clean -qq -y \
    && rm -rf /var/lib/apt/lists/*

FROM ghcr.io/eklipse2k8/magick-build:bullseye-slim AS build-svt

RUN apt-get update \
    && apt-get install -qq -y git nasm \
    && mkdir -p /build \
    && cd /build \
    && git clone -b v1.2.1 --depth 1 https://gitlab.com/AOMediaCodec/SVT-AV1.git \
    && cd SVT-AV1/Build \
    && cmake -DCMAKE_INSTALL_PREFIX=${MAGICK_PATH} -DBUILD_SHARED_LIBS=ON -DCMAKE_BUILD_TYPE=Release .. \
    && cmake --build . -- -j$(nproc) \
    && cmake --install . --prefix ${MAGICK_PATH} \
    && cd / \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -qq -y --auto-remove git nasm \
    && apt-get clean -qq -y \
    && rm -rf /var/lib/apt/lists/*

# FROM rust:1-slim-buster AS build-rav1e

# ENV MAGICK_PATH="/opt/magick"
# ENV DEBIAN_FRONTEND="noninteractive"
# ENV PATH="$HOME/.cargo/bin:$PATH"

# RUN apt-get update \
#     && apt-get install -qq --fix-missing -y ca-certificates openssl libssl-dev \
#       pkg-config locales git nasm \
#     && mkdir -p /build \
#     && cd /build \
#     && git clone -b v0.5.1 --depth 1 https://github.com/xiph/rav1e.git \
#     && cd rav1e; mkdir -p build.libavif \
#     && cargo install cargo-c \
#     && cargo cinstall --release --library-type=staticlib --prefix=${MAGICK_PATH} --destdir build.libavif \
#     && cd / \
#     && apt-get remove -qq -y --auto-remove libssl-dev pkg-config git nasm \
#     && apt-get clean -qq -y \
#     && rm -rf /var/lib/apt/lists/*

FROM ghcr.io/eklipse2k8/magick-build:bullseye-slim AS build-avif

COPY --from=build-dav1d /opt/magick /opt/magick
COPY --from=build-aom /opt/magick /opt/magick
COPY --from=build-gav1 /opt/magick /opt/magick
COPY --from=build-svt /opt/magick /opt/magick
COPY --from=build-sharpyuv /opt/magick /opt/magick
COPY --from=build-yuv /opt/magick /opt/magick

ENV LIBYUV_INCLUDE_DIR="/opt/magick"
ENV _LIBSHARPYUV_INCLUDEDIR="/opt/magick/include"
ENV _LIBSHARPYUV_LIBDIR="/opt/magick/lib"

RUN apt-get update \
    && apt-get install -qq -y curl libjpeg-dev libpng-dev libtiff-dev libgif-dev ninja-build \
    && mkdir -p /build \
    && cd /build \
    && curl -fsSL --http2 https://github.com/AOMediaCodec/libavif/archive/refs/tags/v0.11.1.tar.gz | tar xz \
    && cd libavif-0.11.1 \
    && mkdir build \
    && cd build \
    && cmake -G Ninja -DCMAKE_INSTALL_PREFIX=${MAGICK_PATH} -DCMAKE_BUILD_TYPE=Release -DBUILD_SHARED_LIBS=ON \
      -DAVIF_CODEC_AOM=ON -DAVIF_CODEC_DAV1D=ON -DAVIF_CODEC_RAV1E=OFF -DAVIF_CODEC_LIBGAV1=ON \
      -DAVIF_CODEC_SVT=ON -DAVIF_BUILD_EXAMPLES=OFF -DAVIF_BUILD_APPS=ON -DAVIF_BUILD_TESTS=OFF .. \
    && cmake --build . -- -j$(nproc) \
    && cmake --install . --prefix ${MAGICK_PATH} \
    && cd / \
    && rm -rf /build \
    && ldconfig ${MAGICK_PATH}/lib \
    && apt-get remove -qq -y --auto-remove curl libjpeg-dev libpng-dev libtiff-dev libgif-dev ninja-build \
    && apt-get clean -qq -y \
    && rm -rf /var/lib/apt/lists/*
