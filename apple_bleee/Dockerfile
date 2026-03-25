FROM ubuntu:18.04

RUN apt-get update && \
    apt-get upgrade -y

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
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
    zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /root
RUN git clone https://github.com/hexway/apple_bleee.git

WORKDIR /root/apple_bleee
RUN pip3 install -r requirements.txt && \
    git clone https://github.com/seemoo-lab/owl.git

WORKDIR /root/apple_bleee/owl
RUN git submodule update --init && \
    mkdir build

WORKDIR /root/apple_bleee/owl/build
RUN cmake .. && \
    make && \
    make install

WORKDIR /root/apple_bleee

# Create non-root user
RUN useradd -m -u 1000 appuser && \
    chown -R appuser:appuser /root/apple_bleee
USER appuser

# Add healthcheck (checking if the directory is accessible)
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD test -d /root/apple_bleee || exit 1
