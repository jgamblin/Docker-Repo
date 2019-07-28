FROM ubuntu:18.04

RUN apt-get update && \
    apt-get upgrade -y

RUN apt-get update && \
    apt-get install -y \
    bluetooth \
    bluez \
    cmake \
    g++ \
    gcc \
    git \
    iw \
    libbluetooth-dev \
    libc6-dev \
    libev-dev \
    libjpeg8-dev \
    libnl-3-dev \
    libnl-genl-3-dev \
    libnl-route-3-dev \
    libpcap-dev \
    libudev-dev \
    libusb-1.0-0-dev \
    make \
    net-tools \
    pkg-config \
    python3 \
    python3-pip \
    wget \
    wireless-tools  \
    zlib1g-dev

RUN git clone https://github.com/hexway/apple_bleee.git && \
    cd apple_bleee && \
    pip3 install -r requirements.txt && \
    git clone https://github.com/seemoo-lab/owl.git && \
    cd ./owl && \
    git submodule update --init && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make && \
    make install
