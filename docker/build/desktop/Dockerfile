FROM opendatacam/base-desktop-nvidia-cuda-opencv-gstreamer:1.0

ENV DEBIAN_FRONTEND noninteractive

# Start Darknet Install
RUN git clone --depth 1 -b odc https://github.com/opendatacam/darknet /var/local/darknet

WORKDIR /var/local/darknet

RUN sed -i -e s/GPU=0/GPU=1/ Makefile;
RUN sed -i -e s/CUDNN=0/CUDNN=1/ Makefile;
RUN sed -i -e s/OPENCV=0/OPENCV=1/ Makefile;

RUN make
