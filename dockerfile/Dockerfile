# Use an NVIDIA OpenCL official docker image
#   Refer to the following URL for available tags.
#   https://hub.docker.com/r/nvidia/opencl/tags
FROM nvidia/opencl:devel-ubuntu16.04

ENV DEBCONF_NOWARNINGS yes
ENV DEBIAN_FRONTEND noninteractive
ENV APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE DontWarn

# Update & Upgrade Ubuntu
RUN apt-get -y update \
    && apt-get -y upgrade

# Install dependencies
RUN apt-get install -y git curl cmake g++ qt5-default qt5-qmake
RUN apt-get install -y libboost-dev libboost-program-options-dev \
    libboost-filesystem-dev opencl-headers ocl-icd-libopencl1 \
    ocl-icd-opencl-dev zlib1g-dev

# Set the working directory to /workspace
WORKDIR /workspace

# Test for OpenCL support & compatibility
RUN apt-get install -y clinfo \
    && clinfo

# Clone the leela-zero repogitory
RUN git clone https://github.com/gcp/leela-zero \
    && cd leela-zero \
    && git submodule update --init --recursive \
    && mkdir build

# Make
RUN cd /workspace/leela-zero/build \
    && cmake .. \
    && cmake --build .

# Download the pre-trained weights
RUN cd /workspace/leela-zero/build \
    && curl -O https://zero.sjeng.org/best-network

# Create symbolic link for autogtp
RUN cd /workspace/leela-zero/build/autogtp \
    && ln -s ../leelaz .

# Remove /var/chace
RUN rm -rf /var/cache/*
