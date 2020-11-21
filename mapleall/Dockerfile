FROM ubuntu:18.04
MAINTAINER https://www.openarkcompiler.cn

# Setting up the build environment
RUN sed -i 's/archive.ubuntu.com/mirrors.163.com/g' /etc/apt/sources.list && \
    apt-get -y update && \
    apt install --no-install-recommends -y build-essential clang cmake libelf-dev libssl-dev \
        python3 ca-certificates unzip curl git wget vim libc6-dev \
        autoconf automake autotools-dev bc bison flex gawk gperf libexpat-dev libgmp-dev \
        libmpc-dev libmpfr-dev libtool patchutils texinfo zlib1g-dev libglib2.0-dev libpixman-1-dev pkg-config && \
    rm -rf /var/lib/apt/lists/*

# copy source
RUN git config --global user.email "lxy5266@live.com" && \
    git config --global user.name "ystyle" && \
    git config --global color.ui false && \
    git clone https://gitee.com/openarkcompiler-incubator/mapleall.git

        
WORKDIR /mapleall

RUN sed -i 'N;16 a set -e' tools/setup_tools.sh && \
    cd tools && ./setup_tools.sh

# set proxy
ENV http_proxy=http://192.168.9.191:1081 \ 
    https_proxy=http://192.168.9.191:1081

WORKDIR /
# build riscv-gnu-toolchain
RUN git clone --depth=1 --recursive https://github.com/riscv/riscv-gnu-toolchain && \
    mkdir /usr/riscv64-linux-gnu && export PATH=/usr/riscv64-linux-gnu/bin:$PATH && \
    cd riscv-gnu-toolchain/ && ./configure --prefix=/usr/riscv64-linux-gnu && \
    make -j linux && \
    ln -s /usr/riscv64-linux-gnu/sysroot/lib/ld-linux-riscv64-lp64d.so.1 /lib && \
    cd qemu && ./configure --static --disable-system --target-list=riscv64-linux-user && \
    make -j && cp riscv64-linux-user/qemu-riscv64 /usr/bin



RUN bash -c "source envsetup.sh riscv release && make && make install && make clean"
