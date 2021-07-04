FROM ubuntu:20.04

RUN apt-get update && DEBIAN_FRONTEND="noninteractive" apt-get -y --no-install-recommends install git build-essential wget python3 python3-pip autoconf automake make autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev && rm -rf /var/lib/apt/lists/*
RUN	pip3 install pillow


WORKDIR /root/
RUN git clone https://github.com/riscv/riscv-gnu-toolchain.git && cd /root/riscv-gnu-toolchain/ && ./configure --prefix=/opt/riscv --enable-multilib && make && cd /root/ && rm -rf /root/riscv-gnu-toolchain
# TODO: Shrink image better
ENV PATH="/opt/riscv/bin:${PATH}"

