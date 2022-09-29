ARG ARG_FROM

FROM $ARG_FROM

RUN apt-get update && apt-get upgrade -y

# SuperOdom deps
RUN apt-get update \
    &&  apt-get install -y --no-install-recommends \
        libgoogle-glog-dev \
        libgflags-dev \
        libatlas-base-dev \
        libeigen3-dev \
        libsuitesparse-dev \
        libparmetis-dev \
        cmake \
    && apt-get clean 

# Install Ceres solver
RUN mkdir -p ~/thirdparty-software \
 &&  cd ~/thirdparty-software\
 && git clone https://ceres-solver.googlesource.com/ceres-solver \
 && cd ceres-solver \
 && git checkout 2.1.0 \
 && mkdir build && cd build \
 && cmake .. -DBUILD_TESTING=OFF -DBUILD_DOCUMENTATION=OFF -DBUILD_EXAMPLES=OFF -DBUILD_BENCHMARKS=OFF -DPROVIDE_UNINSTALL_TARGET=OFF \
 && make install -j8 

# Install Gtsam 
RUN cd ~/thirdparty-software\
 && git clone https://github.com/borglab/gtsam.git --branch 4.0.2\
 && cd gtsam \
 && mkdir build && cd build \
 && cmake -DGTSAM_BUILD_WITH_MARCH_NATIVE=OFF -DGTSAM_BUILD_TESTS=OFF -DGTSAM_BUILD_EXAMPLES_ALWAYS=OFF ..\
 && make install -j8 

RUN cd ~/thirdparty-software \
 && git clone http://github.com/strasdat/Sophus.git \
 && cd Sophus && git checkout 97e7161 \
 && mkdir build && cd build && cmake .. -DBUILD_TESTS=OFF \
 && make -j8 &&  make install

RUN  apt install -y libeigen3-dev libfmt-dev python3-catkin-tools ros-melodic-rosmon ros-melodic-rqt-multiplot
#  && sed -i '32,33d' sophus/so2.cpp \
#  && sed -i '31 a unit_complex_ = std::complex<double>(1,0);' sophus/so2.cpp \