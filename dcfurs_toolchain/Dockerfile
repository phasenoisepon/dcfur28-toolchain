FROM ubuntu:20.04 as build

RUN apt update
# apt-get upgrade is not usually reccomended unless there are glaring security issues with the deps
# It creates inconsistent layers with other images that use ubuntu as a base
#RUN    apt-get upgrade -y
RUN    DEBIAN_FRONTEND="noninteractive" apt -y install git build-essential wget python3 python3-pip autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev
RUN    pip3 install pillow


WORKDIR /tmp/
# This is hacky but allows for recursive cloning of submodules 
RUN git clone --depth 1 https://github.com/riscv/riscv-gnu-toolchain.git riscv-gnu-toolchain && \
cd /tmp/riscv-gnu-toolchain/ && \
./configure --prefix=/opt/riscv --enable-multilib && \
make -j16 -l && \
cd /tmp/ && \
rm -rf /tmp/riscv-gnu-toolchain
ENV PATH="/opt/riscv/bin:${PATH}"https://github.com/riscv/riscv-gnu-toolchain.git