FROM nvidia/cuda:8.0-cudnn7-devel-ubuntu16.04
LABEL maintainer="aaronmarkham@fb.com"

# caffe2 install with gpu support

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    cmake \
    git \
    libgflags-dev \
    libgoogle-glog-dev \
    libgtest-dev \
    libiomp-dev \
    libleveldb-dev \
    liblmdb-dev \
    libopencv-dev \
    libopenmpi-dev \
    libprotobuf-dev \
    libsnappy-dev \
    openmpi-bin \
    openmpi-doc \
    protobuf-compiler \
    python-dev \
    python-numpy \
    python-pip \
    python-pydot \
    python-setuptools \
    python-scipy \
    wget \
    && rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir pip==9
RUN pip install --no-cache-dir --upgrade setuptools wheel
RUN pip install --no-cache-dir \
    flask \
    future \
    graphviz \
    hypothesis \
    jupyter \
    matplotlib \
    numpy \
    protobuf \
    pydot \
    python-nvd3 \
    pyyaml \
    requests \
    scikit-image \
    scipy \
    setuptools \
    six \
    tornado

########## INSTALLATION STEPS ###################
#RUN git clone --branch master --recursive https://github.com/caffe2/caffe2.git
RUN git clone --branch master https://github.com/caffe2/caffe2.git
RUN cd caffe2/third_party/aten && git checkout master
RUN cd caffe2 && git pull --recurse-submodules && git submodule sync && git submodule update
RUN ls caffe2/third_party/aten
RUN cd caffe2 && mkdir build && cd build \
    && cmake .. \
    -DCUDA_ARCH_NAME=Manual \
    -DCUDA_ARCH_BIN="35 52 60 61" \
    -DCUDA_ARCH_PTX="61" \
    -DUSE_NNPACK=OFF \
    -DUSE_ROCKSDB=OFF \
    -DUSE_CUDA=ON \
    && make -j"$(nproc)" install \
    && ldconfig \
    && make clean \
    && cd .. \
    && rm -rf build

EXPOSE 8888
VOLUME /notebooks
WORKDIR /notebooks
ENTRYPOINT [ \
	"jupyter", \
	"notebook", \
		"--allow-root", \
		"--no-browser", \
        "--ip='*'" \
]
