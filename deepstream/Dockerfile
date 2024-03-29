
FROM nvcr.io/nvidia/deepstream:6.4-samples-multiarch

# To get video driver libraries at runtime (libnvidia-encode.so/libnvcuvid.so)
ENV NVIDIA_DRIVER_CAPABILITIES $NVIDIA_DRIVER_CAPABILITIES,video
ENV LOGLEVEL="DEBUG"
ENV GST_DEBUG=5
ENV GST_DEBUG_FILE=/app/output/GST_DEBUG.log

RUN apt update && apt install -y \
    libssl3 \
    libssl-dev \
    libgstreamer1.0-0 \
    gstreamer1.0-tools \
    gstreamer1.0-plugins-good \
    gstreamer1.0-plugins-bad \
    gstreamer1.0-plugins-ugly \
    gstreamer1.0-libav \
    libgstreamer-plugins-base1.0-dev \
    libgstrtspserver-1.0-0 \
    libjansson4 \
    libyaml-cpp-dev \
    libjsoncpp-dev \
    protobuf-compiler \
    gcc \
    make \
    git \
    python3

RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/3bf863cc.pub
RUN apt install -y software-properties-common && apt update && add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2204/x86_64/ /"
RUN apt update && apt install -y cuda-toolkit-12-3

RUN apt update
RUN apt install -y python3-gi python3-dev python3-gst-1.0 python3-numpy python3-opencv

# To compile bindings on Ubuntu - 22.04 [use python-3.10, python-3.8 will not work] :
RUN apt install \
    python3-gi \
    python3-dev \
    python3-gst-1.0 \
    python-gi-dev \
    git \
    meson \
    python3 \
    python3-pip \
    python3.10-dev \
    cmake \
    g++ \
    build-essential \
    libglib2.0-dev \
    libglib2.0-dev-bin \
    libgstreamer1.0-dev \
    libgstreamer-plugins-base1.0-dev \
    libtool \
    m4 \
    autoconf \
    automake \
    libgirepository1.0-dev \
    libcairo2-dev -y

RUN bash ./user_additional_install.sh
RUN bash ./install.sh

RUN cd /opt/nvidia/deepstream/deepstream/sources/ \
    && git clone https://github.com/NVIDIA-AI-IOT/deepstream_python_apps.git \
    && cd deepstream_python_apps \
    && git submodule update --init --recursive --progress \
    && apt-get install -y apt-transport-https ca-certificates -y \
    && update-ca-certificates \
    && cd 3rdparty/gstreamer/subprojects/gst-python/ \
    && meson build \
    && meson configure\
    && cd build \
    && ninja \
    && ninja install \
    && cd ../../../../../bindings/ \
    && mkdir build \
    && cd build \
    && cmake .. \
    && make -j$(nproc) \
    && pip3 install ./pyds-*.whl

RUN pip3 install notebook

# RTSP
RUN apt-get install -y libgstrtspserver-1.0-0 gstreamer1.0-rtsp libgirepository1.0-dev gobject-introspection gir1.2-gst-rtsp-server-1.0

COPY . /app

WORKDIR /app

#CMD python3 run.py "file:///app/data/videos/sample_720p.h264"
