# The dockerfile is built after the official NVIDA l4t-jetpack image, but reduced to the necessary
# CUDA and cuDNN, OpenCV libraries only.
#
# See also:
# - https://catalog.ngc.nvidia.com/orgs/nvidia/containers/l4t-base
# - https://catalog.ngc.nvidia.com/orgs/nvidia/containers/l4t-jetpack
# - https://gitlab.com/nvidia/container-images/l4t-jetpack/-/blob/master/Dockerfile.jetpack
FROM nvcr.io/nvidia/l4t-base:35.4.1 AS builder

LABEL maintainer="OpenDataCam <info@opendata.cam>"

ENV DEBIAN_FRONTEND noninteractive

# Install any utils needed for execution
RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Install nvidia-cuda-dev for CUDA developer packages
# Use nvidia-cuda if need CUDA runtime only
RUN apt-get update && apt-get install -y --no-install-recommends \
    nvidia-cuda-dev \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Install nvidia-cudnn8-dev for CuDNN developer packages
# Use nvidia-cudnn8 if need CuDNN runtime only
RUN apt-get update && apt-get install -y --no-install-recommends \
    nvidia-cudnn8-dev \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Install nvidia-opencv-dev for OpenCV developer packages
# Use nvidia-opencv if need OpenCV runtime only
RUN apt-get update && apt-get install -y --no-install-recommends \
    nvidia-opencv-dev \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Install commonly used dependencies
RUN apt-get update \
      && apt-get install --no-install-recommends --no-install-suggests -y gnupg2 ca-certificates \
            git build-essential libopencv-dev wget \
      && rm -rf /var/lib/apt/lists/*

# Build Darknet
RUN git clone --depth 1 -b odc https://github.com/opendatacam/darknet /var/local/darknet
WORKDIR /var/local/darknet
COPY docker/build/xavier/Makefile Makefile
RUN make -j

# --------------------------------------------------------------------------------------------------

FROM nvcr.io/nvidia/l4t-base:35.4.1

LABEL maintainer="OpenDataCam <info@opendata.cam>"

ENV DEBIAN_FRONTEND noninteractive

# Install any utils needed for execution
RUN apt-get update && apt-get install -y --no-install-recommends \
    sudo \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Install nvidia-cuda-dev for CUDA developer packages
# Use nvidia-cuda if need CUDA runtime only
RUN apt-get update && apt-get install -y --no-install-recommends \
    nvidia-cuda \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Install nvidia-cudnn8-dev for CuDNN developer packages
# Use nvidia-cudnn8 if need CuDNN runtime only
RUN apt-get update && apt-get install -y --no-install-recommends \
    nvidia-cudnn8 \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

# Install nvidia-opencv-dev for OpenCV developer packages
# Use nvidia-opencv if need OpenCV runtime only
RUN apt-get update && apt-get install -y --no-install-recommends \
    nvidia-opencv \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

RUN apt-get update \
      && apt-get install --no-install-recommends --no-install-suggests -y libopencv-highgui4.2 \
      && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
    && curl -sL https://deb.nodesource.com/setup_12.x | bash - \
    && apt-get install -y nodejs \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

COPY --from=builder /var/local/darknet /var/local/darknet
