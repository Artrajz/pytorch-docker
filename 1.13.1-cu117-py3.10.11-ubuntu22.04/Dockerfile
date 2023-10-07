FROM nvidia/cuda:11.7.1-base-ubuntu22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHON_VERSION=3.10.11
ENV PYTORCH_VERSION=1.13.1
ENV CUDA_VERSION=cu117
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV PATH /usr/local/bin:$PATH

RUN apt-get update && apt-get upgrade -y && \
    apt-get install --no-install-recommends -y \
    wget \
    build-essential \
    libssl-dev \
    zlib1g-dev \
    libreadline-dev \
    libsqlite3-dev \
    libexpat1-dev \
    liblzma-dev \
    libffi-dev

RUN wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz && \
    tar xzf Python-${PYTHON_VERSION}.tgz && \
    cd Python-${PYTHON_VERSION} && \
    ./configure --enable-optimizations && \
    make altinstall && \
    update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python3.10 1 && \
    update-alternatives --install /usr/bin/python python /usr/local/bin/python3.10 1 && \
    rm -rf /Python-${PYTHON_VERSION}.tgz /Python-${PYTHON_VERSION}

RUN apt-get update && apt-get install --no-install-recommends -y python3-pip && \
    ln -s /usr/bin/pip3 /usr/bin/pip && \
    pip install --no-cache-dir torch==${PYTORCH_VERSION}+${CUDA_VERSION} --extra-index-url https://download.pytorch.org/whl/${CUDA_VERSION}

RUN apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/*

CMD ["python", "--version"]
