ARG ARG_FROM

FROM $ARG_FROM


RUN apt-get update && apt-get upgrade -y

# Install VS Code
RUN wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | apt-key add - && \
add-apt-repository "deb [arch=amd64] https://packages.microsoft.com/repos/vscode stable main"

RUN apt-get update && apt-get install -y \
code && \
apt-get clean && rm -rf /var/lib/apt/lists/*

RUN pip install cpplint

RUN apt-get update -q && \
    apt-get install -y --fix-missing --no-install-recommends \
    # Basics
    ca-certificates wget curl build-essential checkinstall unzip autoconf automake libtool g++ yasm cmake git \
    # google-glog + gflags
    libgoogle-glog-dev \
    # BLAS & LAPACK
    libatlas-base-dev \
    # Eigen3
    libeigen3-dev \
    # SuiteSparse and CXSparse (optional)
    # - If you want to build Ceres as a *static* library (the default)
    #   you can use the SuiteSparse package in the main Ubuntu package
    #   repository:
    libsuitesparse-dev \
    # - However, if you want to build Ceres as a *shared* library, you must
    #   add the following PPA:
    #sudo add-apt-repository ppa:bzindovic/suitesparse-bugfix-1319687
    #sudo apt-get update
    #sudo apt-get install libsuitesparse-dev
    # Sphinx (for documentation)
    python-sphinx

RUN apt-get install -y --no-install-recommends \
    build-essential \
    checkinstall \
    cmake \
    curl \
    g++ \
    gcc \
    git \
    perl \
    pkg-config \
    protobuf-compiler  \
    unzip \
    wget \
    zip \
    zlib1g-dev \
    libdw-dev \
  && apt-get clean

RUN apt-get update && \
        apt-get install -y \
        cmake g++ build-essential git wget unzip yasm pkg-config \
        python3 python-pip python-dev python-numpy python3-dev python3-numpy \
        libcairo-dev libgdk-pixbuf2.0-dev libswscale.dev libgphoto2-dev \
        libdc1394-22-dev libavcodec-dev libavformat-dev libavutil-dev \
        libwebp-dev libpng-dev libtiff-dev libopenexr-dev \
        libswscale-dev libtbb2 libtbb-dev libjpeg-dev libpng-dev \
        libtiff-dev  libavformat-dev libpq-dev \
        libxine2-dev libglew-dev libtiff5-dev zlib1g-dev \
        libpng-dev libavcodec-dev libavformat-dev \
        libavutil-dev libpostproc-dev libswscale-dev libeigen3-dev \
        libtbb-dev libgtk2.0-dev libvtk6-dev libxine2 

# Install system packages
RUN apt-get install -y --no-install-recommends \
    doxygen \
    file \
    gfortran \
    gnupg \
    gstreamer1.0-plugins-good \
    imagemagick \
    libatk-adaptor \
    libatlas-base-dev \
    libavcodec-dev \
    libavformat-dev \
    libavutil-dev \
    libboost-all-dev \
    libcanberra-gtk-module \
    libdc1394-22-dev \
    libeigen3-dev \
    libfaac-dev \
    libfreetype6-dev \
    libgflags-dev \
    libglew-dev \
    libglu1-mesa \
    libglu1-mesa-dev \
    libgoogle-glog-dev \
    libgphoto2-dev \
    libgstreamer1.0-dev \
    libgstreamer-plugins-bad1.0-0 \
    libgstreamer-plugins-base1.0-dev \
    libgtk2.0-dev \
    libgtk-3-dev \
    libhdf5-dev \
    libhdf5-serial-dev \
    libjpeg-dev \
    liblapack-dev \
    libmp3lame-dev \
    libopenblas-dev \
    libopencore-amrnb-dev \
    libopencore-amrwb-dev \
    libopenjp2-7-dev \
    libopenjp2-tools \
    libpng-dev \
    libpostproc-dev \
    libprotobuf-dev \
    libswscale-dev \
    libtbb2 \
    libtbb-dev \
    libtheora-dev \
    libtiff5-dev \
    libv4l-dev \
    libvorbis-dev \
    libx264-dev \
    libxi-dev \
    libxine2-dev \
    libxmu-dev \
    libxvidcore-dev \
    libzmq3-dev \
    v4l-utils \
    x11-apps \
    x264 \
    yasm \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*


WORKDIR /tmp
RUN git clone --depth=1 https://ceres-solver.googlesource.com/ceres-solver && \
    cd ceres-solver && \
    mkdir build && \
    cd build && \
    cmake .. -DBUILD_TESTING=OFF -DBUILD_DOCUMENTATION=OFF -DBUILD_EXAMPLES=OFF -DBUILD_BENCHMARKS=OFF -DPROVIDE_UNINSTALL_TARGET=OFF -DBUILD_SHARED_LIBS=ON && \
    make -j8 && \
    make test && \
    make install

ARG OPENCV_VERSION=3.4.15



RUN mkdir -p /opencv_installation 
WORKDIR /opencv_installation

RUN wget -O opencv.zip https://github.com/opencv/opencv/archive/$OPENCV_VERSION.zip \
    && unzip opencv.zip \
    && rm opencv.zip \
    && wget -O opencv_contrib.zip https://github.com/opencv/opencv_contrib/archive/$OPENCV_VERSION.zip \
    && unzip opencv_contrib.zip \
    && rm opencv_contrib.zip \
    && mkdir opencv-$OPENCV_VERSION/cmake_binary \
    && cd opencv-$OPENCV_VERSION/cmake_binary \
    && cmake -DBUILD_TIFF=ON \
       -DCMAKE_BUILD_TYPE=RELEASE \
       -DCMAKE_INSTALL_PREFIX=/usr/local \
       -DBUILD_opencv_java=OFF \
       -DBUILD_SHARED_LIBS=ON \
       -DWITH_CUDA=ON \
       -DENABLE_FAST_MATH=1 \
       -DCUDA_FAST_MATH=1 \
       -DWITH_CUBLAS=1 \
       -DCUDA_TOOLKIT_ROOT_DIR=/usr/local/cuda-11 \
       ## Should compile for most card
       ## 3.5 binary code for devices with compute capability 3.5 and 3.7,
       ## 5.0 binary code for devices with compute capability 5.0 and 5.2,
       ## 6.0 binary code for devices with compute capability 6.0 and 6.1,
       -DCUDA_ARCH_BIN=7.5 \
       -DCUDA_ARCH_PTX="" \
       ## AVX in dispatch because not all machines have it
       -DCPU_DISPATCH=AVX,AVX2 \
       -DENABLE_PRECOMPILED_HEADERS=OFF \
       -DWITH_OPENGL=OFF \
       -DWITH_OPENCL=OFF \
       -D WITH_CUDNN=ON \
       -D OPENCV_DNN_CUDA=ON \
       -DWITH_QT=OFF \
       -DWITH_IPP=ON \
       -DWITH_TBB=ON \
       -DFORCE_VTK=ON \
       -DWITH_EIGEN=ON \
       -DWITH_V4L=ON \
       -DWITH_XINE=ON \
       -DWITH_GDAL=ON \
       -DWITH_1394=OFF \
       -DWITH_FFMPEG=OFF \
       -DBUILD_PROTOBUF=ON \
       -DBUILD_TESTS=OFF \
       -DBUILD_PERF_TESTS=OFF \
       -DCMAKE_BUILD_TYPE=RELEASE \
       -DOPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-$OPENCV_VERSION/modules \
    .. \
    && export NUMPROC=$(nproc --all) \
    && make -j8 install 
    # && rm -r /opencv-$OPENCV_VERSION \
    # && rm -r /opencv_contrib-$OPENCV_VERSION

WORKDIR /tmp
RUN git clone https://github.com/fixstars/libSGM.git && \
    cd libSGM && mkdir build && cd build && \
    cmake -DBUILD_OPENCV_WRAPPER=ON -DLIBSGM_SHARED=ON .. && \
    make && \
    make install





# Launch terminator
CMD ["terminator"]
