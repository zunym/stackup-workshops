# https://github.com/Microsoft/ELL/blob/master/INSTALL-Ubuntu.md

FROM continuumio/miniconda3:latest

RUN apt-get update \
    && apt-get install -y \
      build-essential \
      cmake \
      curl \
      gcc \
      git \
      libedit-dev \
      zlibc \
      zlib1g \
      zlib1g-dev \
      libopenblas-dev \
      doxygen \
    && apt-get clean all

# LLVM
RUN apt-get update \
    && apt-get install -y \
      llvm-3.9-dev \
    && apt-get clean all

# SWIG
RUN curl -O --location http://prdownloads.sourceforge.net/swig/swig-3.0.12.tar.gz \
    && tar zxvf swig-3.0.12.tar.gz \
    && cd swig-3.0.12 \
    && ./configure --without-pcre \
    && make \
    && make install

# OpenCV
RUN apt-get update \
    && apt-get install -y \
       libgl1-mesa-glx
RUN export PATH="/opt/conda/bin:${PATH}" \
    && conda install --yes --quiet -c conda-forge opencv

# ELL
RUN git clone https://github.com/Microsoft/ELL.git \
    && cd ELL \
    && mkdir -p build \
    && cd build \
    && cmake .. \
    && make \
    && make _ELL_python

ADD import_tiny_yolo.sh /
RUN chmod +x /import_tiny_yolo.sh