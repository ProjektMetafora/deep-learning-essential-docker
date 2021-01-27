FROM nvidia/cuda:9.2-cudnn7-devel-ubuntu18.04

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get upgrade -y && apt-get install -y \
    build-essential \
    cmake \
    git \
    vim \
    wget \
    unzip \
    yasm \
    pkg-config \
    python-pip \
    python3-pip \
    libswscale-dev \
    libtbb2 \
    libtbb-dev \
    libjpeg-dev \
    libpng-dev \
    libtiff-dev \
    libavformat-dev \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

RUN pip3 install --upgrade pip

RUN pip install numpy

# Build OpenCV
WORKDIR /opt
ENV OPENCV_VERSION="4.5.1"
RUN wget https://github.com/opencv/opencv_contrib/archive/${OPENCV_VERSION}.tar.gz --no-check-certificate && tar -xf ${OPENCV_VERSION}.tar.gz && rm ${OPENCV_VERSION}.tar.gz
RUN wget https://github.com/opencv/opencv/archive/${OPENCV_VERSION}.tar.gz --no-check-certificate && tar -xf ${OPENCV_VERSION}.tar.gz && rm ${OPENCV_VERSION}.tar.gz

WORKDIR /opt/opencv-${OPENCV_VERSION}
RUN mkdir build && cd build && \
    cmake 	-D CMAKE_BUILD_TYPE=RELEASE \
    -DBUILD_opencv_java=OFF \
    -DWITH_CUDA=OFF \
    -DWITH_OPENGL=ON \
    -DWITH_OPENCL=ON \
    -DWITH_IPP=ON \
    -DWITH_TBB=ON \
    -DWITH_EIGEN=ON \
    -DWITH_V4L=ON \
    -DBUILD_TESTS=OFF \
    -DBUILD_PERF_TESTS=OFF \
    -D OPENCV_GENERATE_PKGCONFIG=YES \
    -D BUILD_NEW_PYTHON_SUPPORT=ON \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D INSTALL_C_EXAMPLES=OFF \
    -D INSTALL_PYTHON_EXAMPLES=OFF \
    -D OPENCV_EXTRA_MODULES_PATH=/opt/opencv_contrib-${OPENCV_VERSION}/modules \
    -D BUILD_EXAMPLES=OFF /opt/opencv-${OPENCV_VERSION} && \
    make -j7 && \
    make install && \
    ldconfig && \
    rm -rf /opt/opencv*

# Build Darknet
WORKDIR /
RUN git clone https://github.com/AlexeyAB/darknet && cd darknet && git checkout 64efa721ede91cd8ccc18257f98eeba43b73a6af
WORKDIR /darknet
COPY Makefile Makefile

RUN make

CMD ["/bin/bash"]

