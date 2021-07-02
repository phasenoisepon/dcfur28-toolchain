FROM ubuntu:20.04

RUN apt-get update
RUN	apt-get upgrade -y
RUN	DEBIAN_FRONTEND="noninteractive" apt-get -y install git build-essential wget python3 python3-pip autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev
RUN	pip3 install pillow


WORKDIR /root/
RUN git clone https://github.com/riscv/riscv-gnu-toolchain.git
WORKDIR /root/riscv-gnu-toolchain/
RUN ./configure --prefix=/opt/riscv --enable-multilib
RUN make
WORKDIR /root/
# TODO: Shrink image better
RUN rm -rf /root/riscv-gnu-toolchain
ENV PATH="/opt/riscv/bin:${PATH}"

