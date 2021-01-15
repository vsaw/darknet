FROM resinplayground/jetson-nano-cuda-cudnn-opencv:v0.2-slim

ENV DEBIAN_FRONTEND noninteractive

# Start Darknet Install
RUN git clone --depth 1 -b odc https://github.com/opendatacam/darknet /var/local/darknet

WORKDIR /var/local/darknet

# COPY Makefile
COPY docker/build/xavier/Makefile Makefile

RUN make
