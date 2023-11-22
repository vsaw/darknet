FROM nvcr.io/nvidia/cuda:11.4.3-cudnn8-devel-ubuntu20.04 as builder

LABEL maintainer="OpenDataCam <info@opendata.cam>"

ENV DEBIAN_FRONTEND noninteractive

# Install commonly used dependencies
RUN apt-get update \
      && apt-get install --no-install-recommends --no-install-suggests -y gnupg2 ca-certificates \
            git build-essential libopencv-dev wget pkg-config \
      && rm -rf /var/lib/apt/lists/*

# Build Darknet
RUN git clone --depth 1 https://github.com/opendatacam/darknet /var/local/darknet
WORKDIR /var/local/darknet
RUN sed -i -e s/GPU=0/GPU=1/ Makefile;
RUN sed -i -e s/CUDNN=0/CUDNN=1/ Makefile;
RUN sed -i -e s/OPENCV=0/OPENCV=1/ Makefile;
RUN make -j

# --------------------------------------------------------------------------------------------------

FROM nvcr.io/nvidia/cuda:11.4.3-cudnn8-runtime-ubuntu20.04

LABEL maintainer="OpenDataCam <info@opendata.cam>"

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update \
      && apt-get install --no-install-recommends --no-install-suggests -y libopencv-highgui4.2 \
      && rm -rf /var/lib/apt/lists/* \
      && apt-get clean

RUN apt-get update \
    && apt-get install --no-install-recommends --no-install-suggests -y curl \
    && curl -sL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get install --no-install-recommends --no-install-suggests -y nodejs \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

COPY --from=builder /var/local/darknet /var/local/darknet
