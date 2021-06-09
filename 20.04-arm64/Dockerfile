FROM ubuntu:20.04

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
    ca-certificates \
    cmake \
    dirmngr \
    freeglut3-dev \
    gnupg2 \
    gpg-agent \
    git \
    gcc-7 \
    g++-7 \
    iputils-ping \
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
    libleveldb-dev \
    liblmdb-dev \
    libsnappy-dev \
    libass-dev \
    libdrm-dev \
    libvorbis-dev \
    libogg-dev \
    libx11-dev \
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
    libevent-dev \
    libcurl4-openssl-dev \
    libopenexr-dev \
    librtmp-dev \
    mesa-common-dev \
    mercurial \
    net-tools \
    nasm \
    openssh-server \
    pkg-config \
    python3 \
    python3-pip \
    sudo \
    software-properties-common \
    texinfo \
    unzip \
    vim \
    wget \
    xauth \
    xclip \
    xorg-dev \
    yasm \
    zlib1g-dev \
    zip \
    && apt autoremove -y python2 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*

#    ctags \
#    gawk \
#    id-utils \
#    graphviz \
#    libperl-dev \
#    python3-dev \
#    virtualenv \
#    libx264-dev \
#    libx265-dev \
#    libva-dev \
#    libatlas-base-dev \
#    libboost-all-dev \
#    libhdf5-serial-dev \

# Change gcc and g++ to 7
RUN rm /usr/bin/gcc \
    && rm /usr/bin/g++ \
    && ln -s /usr/bin/gcc-7 /usr/bin/gcc \
    && ln -s /usr/bin/g++-7 /usr/bin/g++

RUN mkdir -p /workspace/tmp

# Install ffmpeg
RUN cd /workspace/tmp/ \
    && wget -O ffmpeg-4.3.2.tar.bz2 \
    ftp://192.168.0.100/corerain-public-tools/docker-libs/ffmpeg-4.3.2.tar.bz2 \
    && tar xvf ffmpeg-4.3.2.tar.bz2 \
    && cd ffmpeg-4.3.2/ \
    && ./configure \
    --disable-static \
    --enable-shared \
    --enable-gpl \
    && make -j$MAKE_THREADS \
    && make install \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install OpenCV for C and C++
RUN cd /workspace/tmp/ \
    && wget -O opencv-3.4.zip \
    ftp://192.168.0.100/corerain-public-tools/docker-libs/opencv-3.4.zip \
    && unzip opencv-3.4.zip \
    && wget -O opencv_contrib-3.4.zip \
    ftp://192.168.0.100/corerain-public-tools/docker-libs/opencv_contrib-3.4.zip \
    && unzip opencv_contrib-3.4.zip \
    && cd opencv-3.4 \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-3.4/modules \
    -DCMAKE_BUILD_TYPE=release \
    -DBUILD_SHARED_LIBS=ON \
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

# Install glog gtest gflags protobuf eigen
RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    protobuf-compiler \
    libprotobuf-dev \
    libeigen3-dev \
    libgflags-dev \
    libgtest-dev \
    libgoogle-glog-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*

# Install rga 06fc7c4
RUN apt-get update -y \
    && apt-get install -y --no-install-recommends \
    ninja-build \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
           /tmp/* \
           /var/tmp/* \
           /usr/share/doc/*
RUN pip3 install --no-cache-dir meson
RUN cd /workspace/tmp/ \
    && wget -O linux-rga-master.zip \
    ftp://192.168.0.100/corerain-public-tools/docker-libs/linux-rga-master.zip \
    && unzip linux-rga-master.zip \
    && cd linux-rga-master \
    && sed -i '46 d' RockchipRga.cpp \
    && sed -i '11 a #ifndef _RockchipRga_' RockchipRga.h \
    && sed -i '12 a #define _RockchipRga_' RockchipRga.h \
    && sed -i '$ a #endif' RockchipRga.h \
    && meson build \
    && cd build \
    && ninja \
    && cp librga.so.2.0.0 /usr/local/lib \
    && mkdir -p /usr/local/include/librga \
    && cp ../*.h /usr/local/include/librga \
    && cd /usr/local/lib \
    && ln -s librga.so.2.0.0 librga.so.2 \
    && ln -s librga.so.2 librga.so \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Install mpp 3c5dc7f
RUN cd /workspace/tmp/ \
    && wget -O mpp-release.zip \
    ftp://192.168.0.100/corerain-public-tools/docker-libs/mpp-release.zip \
    && unzip mpp-release.zip \
    && cd mpp-release \
    && sed -i '348,351 d' CMakeLists.txt \
    && sed -i '347 a install(DIRECTORY\ ${CMAKE_CURRENT_SOURCE_DIR}/inc/' CMakeLists.txt \
    && sed -i '348 a \        DESTINATION\ "${CMAKE_INSTALL_INCLUDEDIR}/rockchip"' CMakeLists.txt \
    && sed -i '349 a \        FILES_MATCHING\ PATTERN\ "*.h*")' CMakeLists.txt \
    && sed -i '350 a install(DIRECTORY\ ${CMAKE_CURRENT_SOURCE_DIR}/utils/' CMakeLists.txt \
    && sed -i '351 a \        DESTINATION\ "${CMAKE_INSTALL_INCLUDEDIR}/utils"' CMakeLists.txt \
    && sed -i '352 a \        FILES_MATCHING\ PATTERN\ "*.h*")' CMakeLists.txt \
    && sed -i '353 a install(DIRECTORY\ ${CMAKE_CURRENT_SOURCE_DIR}/osal/inc/' CMakeLists.txt \
    && sed -i '354 a \        DESTINATION\ "${CMAKE_INSTALL_INCLUDEDIR}/osal"' CMakeLists.txt \
    && sed -i '355 a \        FILES_MATCHING\ PATTERN\ "*.h*")' CMakeLists.txt \
    && sed -i '356 a install(DIRECTORY\ ${CMAKE_CURRENT_SOURCE_DIR}/mpp/base/inc/' CMakeLists.txt \
    && sed -i '357 a \        DESTINATION\ "${CMAKE_INSTALL_INCLUDEDIR}/mpp/base"' CMakeLists.txt \
    && sed -i '358 a \        FILES_MATCHING\ PATTERN\ "*.h*")' CMakeLists.txt \
    && sed -i '359 a install(DIRECTORY\ ${CMAKE_CURRENT_SOURCE_DIR}/mpp/codec/inc/' CMakeLists.txt \
    && sed -i '360 a \        DESTINATION\ "${CMAKE_INSTALL_INCLUDEDIR}/mpp/codec"' CMakeLists.txt \
    && sed -i '361 a \        FILES_MATCHING\ PATTERN\ "*.h*")' CMakeLists.txt \
    && sed -i '362 a install(DIRECTORY\ ${CMAKE_CURRENT_SOURCE_DIR}/mpp/hal/inc/' CMakeLists.txt \
    && sed -i '363 a \        DESTINATION\ "${CMAKE_INSTALL_INCLUDEDIR}/mpp/hal"' CMakeLists.txt \
    && sed -i '364 a \        FILES_MATCHING\ PATTERN\ "*.h*")' CMakeLists.txt \
    && sed -i '365 a install(DIRECTORY\ ${CMAKE_CURRENT_SOURCE_DIR}/mpp/vproc/inc/' CMakeLists.txt \
    && sed -i '366 a \        DESTINATION\ "${CMAKE_INSTALL_INCLUDEDIR}/mpp/vproc"' CMakeLists.txt \
    && sed -i '367 a \        FILES_MATCHING\ PATTERN\ "*.h*")' CMakeLists.txt \
    && cd build/linux/aarch64 \
    && sed -i 's/aarch64-linux-gnu-gcc/gcc/g' arm.linux.cross.cmake \
    && sed -i 's/aarch64-linux-gnu-g++/g++/g' arm.linux.cross.cmake \
    && sed -i '28 a \      -DCMAKE_INSTALL_PREFIX=./install \\' make-Makefiles.bash \
    && bash make-Makefiles.bash \
    && make -j$MAKE_THREADS \
    && make install \
    && cp osal/libosal.a utils/libutils.a /usr/local/lib \
    && cp -r install/lib/* /usr/local/lib \
    && mkdir -p /usr/local/include/mpp \
    && cp -r install/include/* /usr/local/include/mpp \
    && ldconfig \
    && rm -rf /workspace/tmp/*

# Finished
# To build an image:
# docker build -t <image_name>:<image_tag> -f <Dockerfile_name> .
