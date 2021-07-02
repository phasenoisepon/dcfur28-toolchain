FROM dcfurs_toolchain:latest

WORKDIR /root/
RUN git clone https://github.com/defconfurs/dcfurs-badge-dc28.git
WORKDIR /root/dcfurs-badge-dc28
RUN python3 ./make.py clean
RUN echo "${PATH}"
RUN python3 ./make.py
