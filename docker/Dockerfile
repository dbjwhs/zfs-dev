FROM ubuntu:latest

# Install build dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    autoconf \
    automake \
    libtool \
    gawk \
    libblkid-dev \
    uuid-dev \
    libudev-dev \
    libssl-dev \
    zlib1g-dev \
    libaio-dev \
    libattr1-dev \
    libelf-dev \
    python3 \
    python3-dev \
    python3-setuptools \
    python3-cffi \
    git \
    libtirpc-dev \
    linux-headers-generic \
    sudo \
    dos2unix \
    ksh \
    && rm -rf /var/lib/apt/lists/*

# Clone OpenZFS
RUN git clone https://github.com/openzfs/zfs.git /zfs

WORKDIR /zfs

# Build OpenZFS
RUN sh autogen.sh && \
    ./configure && \
    make -j$(nproc) && \
    chmod +x /zfs/tests/runfiles/linux.run && \
    dos2unix /zfs/tests/runfiles/linux.run

# Set up a workspace directory
WORKDIR /workspace