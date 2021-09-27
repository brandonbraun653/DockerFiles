FROM ubuntu:21.04
LABEL maintainer="Brandon Braun <brandonbraun653@gmail.com>"
LABEL Description="Image for building and debugging arm-embedded projects from git"

# Prevent tzdata interaction in build below
ENV TZ=America/Phoenix
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# RUN useradd -ms /bin/bash dev
# USER root

# Install any needed packages specified in requirements.txt
RUN apt-get update && \
    apt-get install -y \
# Development packages
      autotools-dev \
      build-essential \
      bzip2 \
      cmake \
      g++ \
      gdb-multiarch \
      git \
      libboost-all-dev\
      libbz2-dev \
      libicu-dev \
      protobuf-compiler \
      python-dev \
      wget

RUN mkdir -p libraries
RUN mkdir -p miniconda3
RUN mkdir -p toolchain

# miniconda
# WORKDIR /miniconda3
# RUN wget -qO- https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
# RUN bash miniconda.sh -b -u -p /miniconda3
# RUN rm -rf miniconda.sh
# RUN ./bin/conda init bash
# RUN ./bin/conda init zsh

# ENV PATH "/miniconda3/bin:$PATH"

# gcc-arm-none-eabi
WORKDIR /toolchain
#RUN wget -qO- https://developer.arm.com/-/media/Files/downloads/gnu-rm/10.3-2021.07/gcc-arm-none-eabi-10.3-2021.07-x86_64-linux.tar.bz2 | tar -xj
RUN wget -qO- https://developer.arm.com/-/media/Files/downloads/gnu-rm/9-2020q2/gcc-arm-none-eabi-9-2020-q2-update-x86_64-linux.tar.bz2 | tar -xj

ENV PATH "/toolchain/gcc-arm-none-eabi-9-2020-q2-update/bin:$PATH"
