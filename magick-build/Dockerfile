FROM debian:buster-slim

ENV LANG="en_US.UTF-8"
ENV LC_ALL="en_US.UTF-8"
ENV LANGUAGE="en_US.UTF-8"
ENV LC_CTYPE="en_US.UTF-8"
ENV MAGICK_PATH="/opt/magick"
ENV LD_LIBRARY_PATH="/opt/magick/lib:/opt/magick/lib/aarch64-linux-gnu:/opt/magick/lib/x86_64-linux-gnu"
ENV PATH="$PATH:/opt/magick/bin"
ENV PKG_CONFIG_PATH="/opt/magick/lib/pkgconfig"

RUN apt-get update \
    && apt-get install -qq --fix-missing -y ca-certificates openssl build-essential libssl-dev \
      pkg-config curl locales \
    && sed -i 's/^# *\(en_US.UTF-8\)/\1/' /etc/locale.gen \
    && locale-gen \
    && dpkg-reconfigure --frontend noninteractive locales \
    && update-locale "LC_ALL=en_US.UTF-8" \
    && mkdir -p /build \
    && cd /build \
    && curl -fsSL --http2 https://github.com/Kitware/CMake/releases/download/v3.25.2/cmake-3.25.2.tar.gz | tar xz \
    && cd cmake-3.25.2 \
    && ./bootstrap \
    && make -j$(nproc) \
    && make install \
    && cd / \
    && rm -rf /build \
    && ldconfig \
    && apt-get remove -y --auto-remove curl libssl-dev \
    && rm -rf /var/lib/apt/lists/*;
