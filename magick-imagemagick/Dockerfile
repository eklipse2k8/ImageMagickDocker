FROM ghcr.io/eklipse2k8/magick-build-heif:bullseye-slim AS magick-build

ENV LD_LIBRARY_PATH="/opt/magick/lib:/opt/magick/lib/aarch64-linux-gnu:/opt/magick/lib/x86_64-linux-gnu"
ENV PKG_CONFIG_PATH="/opt/magick/lib/pkgconfig"
ENV MAGICK_PATH="/opt/magick"

COPY --from=ghcr.io/eklipse2k8/magick-build-exiv2:bullseye-slim /opt/magick /opt/magick

RUN apt-get update \
    && apt-get install -y curl libde265-dev libjpeg62-turbo-dev libx265-dev libjpeg-dev libpng-dev libpng16-16 \
      libjpeg-dev libtiff-dev libgif-dev libzip-dev libzstd-dev libltdl-dev liblzma-dev zlib1g-dev libjbig-dev \
      libxml2-dev libxml2-utils libopenjp2-7-dev liblcms2-dev libexpat1-dev \
    && mkdir -p /build \
    && cd /build \
    && curl -fsSL --http2 https://github.com/ImageMagick/ImageMagick/archive/refs/tags/7.1.0-58.tar.gz | tar xz \
    && cd ImageMagick-7.1.0-58 \
    && ldconfig ${MAGICK_PATH}/lib \
    && JXL_CFLAGS="-I/opt/magick/include" LDFLAGS="-L/opt/magick/lib" \
      HEIF_CFLAGS="-I/opt/magick/include" \
      ./configure  \
      --with-magick-plus-plus=no --with-perl=no --disable-docs \
      --with-modules \
      --with-webp \
      --with-jbig \
      --disable-static --without-x --without-wmf --with-gslib --prefix=/opt/magick \
    && make -j$(nproc) \
    && make install -j$(nproc) \
    && cd / \
    && rm -rf /build \
    && rm /opt/magick/lib/libaom.a /opt/magick/lib/libgav1.a /opt/magick/lib/libyuv.a /opt/magick/lib/libjxl_threads.a \
      /opt/magick/lib/libjxl.a /opt/magick/lib/libjxl_dec.a /opt/magick/lib/libhwy.a /opt/magick/lib/libhwy_test.a \
    && rm -rf /opt/magick/include \
    && rm -rf /opt/magick/share/WebP \
    && rm -rf /opt/magick/share/man \
    && rm -rf /opt/magick/lib/cmake \
    && rm -rf /opt/magick/lib/pkgconfig

FROM debian:bullseye-slim

LABEL org.opencontainers.image.source https://github.com/eklipse2k8/ImageMagickDocker

ENV LANG="en_US.UTF-8"
ENV LC_ALL="en_US.UTF-8"
ENV LANGUAGE="en_US.UTF-8"
ENV LC_CTYPE="en_US.UTF-8"
ENV MAGICK_PATH="/opt/magick"
ENV LD_LIBRARY_PATH="/opt/magick/lib:/opt/magick/lib/aarch64-linux-gnu:/opt/magick/lib/x86_64-linux-gnu"
ENV PATH="$PATH:/opt/magick/bin"
ENV PKG_CONFIG_PATH="/opt/magick/lib/pkgconfig"

COPY --from=magick-build /opt/magick /opt/magick

RUN apt-get update \
    && apt-get install -qq --fix-missing -y ca-certificates openssl  \
      libde265-0 libjpeg62-turbo x265 libpng16-16 libtiff5 libgif7 zip zstd libltdl7 \
      locales libzip4 libgomp1 libopenjp2-7 libxml2 liblcms2-2 libexpat1 \
    && sed -i 's/^# *\(en_US.UTF-8\)/\1/' /etc/locale.gen \
    && locale-gen \
    && dpkg-reconfigure --frontend noninteractive locales \
    && update-locale "LC_ALL=en_US.UTF-8" \
    && rm -rf /var/lib/apt/lists/* \
    && ldconfig ${MAGICK_PATH}/lib \
    && chmod -R -w /opt
