FROM nvidia/cuda:11.8.0-base-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHON_VERSION=3.10.11
ENV PYTHON_MAJOR=3
ENV PYTHON_MINOR=10
ENV PYTORCH_VERSION=2.2.1
ENV CUDA_VERSION=cu118
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PATH /usr/local/bin:$PATH

RUN apt-get update && apt-get upgrade -y && \
    apt-get install -y --no-install-recommends ca-certificates && \
    apt-get install --no-install-recommends -y \
    wget \
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libreadline-dev \
    libsqlite3-dev \
    libexpat1-dev \
    liblzma-dev \
    libffi-dev \
    libbz2-dev && \
    apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/*

RUN wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz && \
    tar xzf Python-${PYTHON_VERSION}.tgz && \
    cd Python-${PYTHON_VERSION} && \
    ./configure --enable-optimizations && \
    make -j$(nproc) altinstall && \
    update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python${PYTHON_MAJOR}.${PYTHON_MINOR} 1 && \
    update-alternatives --install /usr/bin/python python /usr/local/bin/python${PYTHON_MAJOR}.${PYTHON_MINOR} 1 && \
    rm -rf /Python-${PYTHON_VERSION}.tgz /Python-${PYTHON_VERSION}

RUN python -m ensurepip && \
    python -m pip install --upgrade pip && \
    ln -sf $(which pip) /usr/local/bin/pip3 && \
    pip install wheel

ENV CFLAGS="-I/usr/local/include/python${PYTHON_MAJOR}.${PYTHON_MINOR}/"

RUN pip install --no-cache-dir torch==${PYTORCH_VERSION}+${CUDA_VERSION} --extra-index-url https://download.pytorch.org/whl/${CUDA_VERSION}
