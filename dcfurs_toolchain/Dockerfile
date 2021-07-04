FROM ubuntu:20.04 as build

RUN apt update
# apt-get upgrade is not usually reccomended unless there are glaring security issues with the deps
# It creates inconsistent layers with other images that use ubuntu as a base
#RUN	apt-get upgrade -y
RUN	DEBIAN_FRONTEND="noninteractive" apt -y install git build-essential wget python3 python3-pip autoconf automake autotools-dev curl libmpc-dev libmpfr-dev libgmp-dev gawk bison flex texinfo gperf libtool patchutils bc zlib1g-dev libexpat-dev
RUN	pip3 install pillow

RUN mkdir /build
WORKDIR /build
# This is hacky but allows for recursive cloning of submodules 
RUN git clone --depth 2 -j8 https://github.com/riscv/riscv-gnu-toolchain.git riscv-gnu-toolchain
WORKDIR /build/riscv-gnu-toolchain/
# Tell the git submodule stuff to only clone in a shallow form. Helps with compile times.
#RUN sed -i 's/git submodule update/git submodule update --quiet --init --depth=1 --remote/g' Makefile.in
RUN ./configure --prefix=/opt/riscv --enable-multilib
# GH runners have 2 cores
RUN make -j 3 -l
WORKDIR /root/
# TODO: Shrink image bettertmp
#RUN rm -rf /root/riscv-gnu-toolchain
#RUN mkdir /opt/riscv_libs
#RUN ldd /opt/riscv/bin/riscv64-unknown-elf-gcc | grep "=> /" | awk '{print $3}' | xargs -I '{}' cp -v '{}' /opt/riscv_libs
#ENV PATH="/opt/riscv/bin:${PATH}"

# Edge image
FROM debian:stable-slim as dev
ENV PATH="$PATH:/opt/riscv/bin"
# Get the DCFURS Stuff
RUN apt update -y && \
    apt install -y python3 python3-pillow libmpc-dev dfu-util curl

RUN mkdir -p /opt/riscv/bin
COPY --from=build /opt/riscv/bin/riscv64-unknown-elf-gcc /opt/riscv/bin/riscv64-unknown-elf-gcc
COPY --from=build /opt/riscv/bin/riscv64-unknown-elf-objcopy /opt/riscv/bin/riscv64-unknown-elf-objcopy
COPY --from=build /opt/riscv/lib /opt/riscv/lib
COPY --from=build /opt/riscv/libexec /opt/riscv/libexec
COPY --from=build /opt/riscv/riscv64-unknown-elf /opt/riscv/riscv64-unknown-elf
RUN curl -L https://api.github.com/repos/defconfurs/dcfurs-badge-dc28/tarball | tar xzvf - --one-top-level="dcfurs-badge-dc28" --strip-components 1 -C /opt
WORKDIR /opt/dcfurs-badge-dc28
RUN python3 ./make.py clean
RUN python3 ./make.py
#RUN python3 ./make.py upload