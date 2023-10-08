FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive
ENV PYTHON_VERSION=3.10.11
ENV PYTORCH_VERSION=1.13.1
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
    libffi-dev

RUN wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz && \
    tar xzf Python-${PYTHON_VERSION}.tgz && \
    cd Python-${PYTHON_VERSION} && \
    ./configure --enable-optimizations && \
    make altinstall && \
    PYTHON_MAJOR=$(echo $PYTHON_VERSION | cut -d'.' -f1) && \
    PYTHON_MINOR=$(echo $PYTHON_VERSION | cut -d'.' -f2) && \
    update-alternatives --install /usr/bin/python3 python3 /usr/local/bin/python${PYTHON_MAJOR}.${PYTHON_MINOR} 1 && \
    update-alternatives --install /usr/bin/python python /usr/local/bin/python${PYTHON_MAJOR}.${PYTHON_MINOR} 1 && \
    rm -rf /Python-${PYTHON_VERSION}.tgz /Python-${PYTHON_VERSION}

ENV CFLAGS="-I/usr/local/include/python${PYTHON_MAJOR}.${PYTHON_MINOR}/"

RUN apt-get update && apt-get install --no-install-recommends -y python3-pip && \
    ln -sf /usr/bin/pip3 /usr/bin/pip && \
    ARCH=$(uname -m) && \
    if [ "$ARCH" = "x86_64" ]; then \
        PYTORCH_SUFFIX="+cpu"; \
    elif [ "$ARCH" = "aarch64" ]; then \
        PYTORCH_SUFFIX=""; \
    else \
        echo "Unsupported architecture"; exit 1; \
    fi && \
    pip install --no-cache-dir torch==${PYTORCH_VERSION}$PYTORCH_SUFFIX --extra-index-url https://download.pytorch.org/whl/cpu

RUN apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/*

CMD ["python", "--version"]
