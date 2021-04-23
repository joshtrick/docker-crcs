FROM ubuntu:18.04

MAINTAINER hao jiang (hao.jiang@corerain.com)
ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NOWARNINGS yes
ARG MAKE_THREADS=4

RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    autoconf \
    automake \
    build-essential \
    curl \
    ctags \
    ca-certificates \
    dirmngr \
    freeglut3-dev \
    gnupg2 \
    gpg-agent \
    graphviz \
    gawk \
    git \
    id-utils \
    libssl-dev \
    libpq-dev \
    libtool \
    libglib2.0-0 \
    libsm6 \
    libxext6 \
    libxrender1 \
    libgtk-3-dev \
    libcairo2-dev \
    libmtdev-dev \
    libxkbcommon-x11-0 \
    libglu1-mesa-dev \
    libatlas-base-dev \
    libboost-all-dev \
    libhdf5-serial-dev \
    libleveldb-dev \
    liblmdb-dev \
    libsnappy-dev \
    libass-dev \
    libva-dev \
    libdrm-dev \
    libvorbis-dev \
    libogg-dev \
    libx11-dev \
    libperl-dev \
    libpciaccess-dev \
    libpciaccess0 \
    libfreetype6-dev \
    libsdl2-dev \
    libvdpau-dev \
    libxcb1-dev \
    libxcb-shm0-dev \
    libxcb-xfixes0-dev \
    libv4l-dev \
    libasound2-dev \
    libx264-dev \
    libx265-dev \
    libevent-dev \
    libcurl4-openssl-dev \
    mesa-common-dev \
    mercurial \
    net-tools \
    nasm \
    openssh-server \
    pkg-config \
    python3 \
    python3-dev \
    python3-pip \
    sudo \
    software-properties-common \
    texinfo \
    unzip \
    virtualenv \
    vim \
    wget \
    xauth \
    xclip \
    xorg-dev \
    yasm \
    zlib1g-dev \
    zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*

RUN mkdir -p /workspace/tmp

# Install cmake
RUN cd /workspace/tmp/ \
    && wget -O cmake-3.14.7-Linux-x86_64.tar.gz \
    https://cmake.org/files/v3.14/cmake-3.14.7-Linux-x86_64.tar.gz \
    && tar xzf cmake-3.14.7-Linux-x86_64.tar.gz \
    && mv cmake-3.14.7-Linux-x86_64 /opt/cmake-3.14.7 \
    && ln -sf /opt/cmake-3.14.7/bin/* /usr/bin/ \
    && rm -rf /workspace/tmp/*

# Install gmmlib
RUN cd /workspace/tmp/ \
    && wget -O gmmlib-master.zip \
    https://codeload.github.com/intel/gmmlib/zip/refs/heads/master \
    && unzip gmmlib-master.zip \
    && cd gmmlib-master \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DCMAKE_BUILD_TYPE=release \
    -DARCH=64 \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install libva
RUN cd /workspace/tmp/ \
    && wget -O libva-master.zip \
    https://codeload.github.com/intel/libva/zip/refs/heads/master \
    && unzip libva-master.zip \
    && cd libva-master \
    && ./autogen.sh \
    && ./configure \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install Media Driver
RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    autoconf \
    libtool \
    libdrm-dev \
    xorg \
    xorg-dev \
    openbox \
    libx11-dev \
    libgl1-mesa-glx \
    libgl1-mesa-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*

RUN cd /workspace/tmp/ \
    && wget -O media-driver-master.zip \
    https://codeload.github.com/intel/media-driver/zip/refs/heads/master \
    && unzip media-driver-master.zip \
    && cd media-driver-master \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/* \
    && echo "export LIBVA_DRIVER_NAME=iHD" >> ~/.bashrc \
    && echo "export LIBVA_DRIVERS_PATH=/usr/lib/x86_64-linux-gnu/dri" >> ~/.bashrc

# Install ffmpeg
RUN cd /workspace/tmp/ \
    && wget -O ffmpeg-4.3.2.tar.bz2 \
    https://ffmpeg.org/releases/ffmpeg-4.3.2.tar.bz2 \
    && tar xvf ffmpeg-4.3.2.tar.bz2 \
    && cd ffmpeg-4.3.2/ \
    && ./configure \
    --disable-yasm \
    --enable-shared \
    --enable-gpl \
    --enable-libx264 \
    --enable-libx265 \
    --enable-libmfx \
    --enable-vaapi \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install OpenCV for C and C++
RUN cd /workspace/tmp/ \
    && wget -O opencv-3.4.zip \
    https://codeload.github.com/opencv/opencv/zip/3.4 \
    && unzip opencv-3.4.zip \
    && wget -O opencv_contrib-3.4.zip \
    https://codeload.github.com/opencv/opencv_contrib/zip/refs/heads/3.4 \
    && unzip opencv_contrib-3.4.zip \
    && cd opencv-3.4 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-3.4/modules \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    -DBUILD_PROTOBUF=OFF \
    -DBUILD_opencv_dnn=OFF \
    -DBUILD_TESTS=OFF \
    -DBUILD_PERF_TESTS=OFF \
    -DBUILD_EXAMPLES=OFF \
    -DBUILD_opencv_apps=OFF \
    -DWITH_CUBLAS=OFF \
    -DWITH_CUDA=OFF \
    -DWITH_CUFFT=OFF \
    -DWITH_OPENCL=OFF \
    -DWITH_OPENCLAMDBLAS=OFF \
    -DWITH_OPENCLAMDFFT=OFF \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install protobuf
RUN cd /workspace/tmp/ \
    && wget -O protobuf-3.5.1.1.zip \
    https://codeload.github.com/protocolbuffers/protobuf/zip/3.5.1.1 \
    && unzip protobuf-3.5.1.1.zip \
    && cd protobuf-3.5.1.1 \
    && ./autogen.sh \
    && ./configure \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install gtest
RUN cd /workspace/tmp/ \
    && wget -O googletest-release-1.8.0.zip \
    https://codeload.github.com/google/googletest/zip/release-1.8.0 \
    && unzip googletest-release-1.8.0.zip \
    && cd googletest-release-1.8.0 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -G"Unix Makefiles" \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    -DBUILD_SHARED_LIBS=true \
    -DBUILD_STATIC_LIBS=true \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install gflags
RUN cd /workspace/tmp/ \
    && wget -O gflags-2.2.2.zip \
    https://codeload.github.com/gflags/gflags/zip/v2.2.2 \
    && unzip gflags-2.2.2.zip \
    && cd gflags-2.2.2 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DCMAKE_BUILD_TYPE=release \
    -DCMAKE_INSTALL_PREFIX=/usr/local/ \
    -DBUILD_SHARED_LIBS=true \
    -DBUILD_STATIC_LIBS=true \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install glog
RUN cd /workspace/tmp/ \
    && wget -O glog-0.3.4.zip \
    https://codeload.github.com/google/glog/zip/v0.3.4 \
    && unzip glog-0.3.4.zip \
    && cd glog-0.3.4 \
    && ./configure \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Change download sources
RUN cp /etc/apt/sources.list /etc/apt/sources.list.bk \
    && sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list

# Finished
# To build an image:
# docker build -t <image_name>:<image_tag> -f <Dockerfile_name> .
