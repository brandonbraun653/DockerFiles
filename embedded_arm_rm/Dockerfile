FROM ubuntu:20.04
LABEL maintainer="Brandon Braun <brandonbraun653@gmail.com>"
LABEL Description="Image for building and debugging arm-embedded projects from git"

# Prevent tzdata interaction in build below
ENV TZ=America/Phoenix
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get clean && \
    apt-get update && \
    apt-get install -y \
      autotools-dev \
      build-essential \
      bzip2 \
      g++ \
      gdb-multiarch \
      git \
      lsb-release \
      minicom \
      protobuf-compiler \
      software-properties-common \
      usbutils \
      wget && \
      apt-get clean

# Get the latest CMake for this image
RUN wget -O - https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/nul
RUN apt-add-repository "deb https://apt.kitware.com/ubuntu/ $(lsb_release -cs) main"
RUN apt-get update && \
    apt-get install -y cmake

WORKDIR /home/vscode
RUN mkdir -p libraries
RUN mkdir -p miniconda3
RUN mkdir -p toolchain

# miniconda
#RUN wget -qO- https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh
COPY ./Miniconda3-latest-Linux-x86_64.sh /miniconda3/miniconda.sh
RUN bash /miniconda3/miniconda.sh -b -u -p /miniconda3
RUN rm -rf /miniconda3/miniconda.sh
RUN /miniconda3/bin/conda init bash
RUN /miniconda3/bin/conda init zsh
ENV PATH "/miniconda3/bin:$PATH"

# gcc-arm-none-eabi
# RUN wget https://developer.arm.com/-/media/Files/downloads/gnu-rm/10.3-2021.07/gcc-arm-none-eabi-10.3-2021.07-x86_64-linux.tar.bz2 | tar -xj
# COPY gcc-arm-none-eabi-10.3-2021.07-x86_64-linux.tar.bz2 /toolchain/gcc-arm-none-eabi-10.3-2021.07-x86_64-linux.tar.bz2
# RUN tar -xf toolchain/gcc-arm-none-eabi-10.3-2021.07-x86_64-linux.tar.bz2

# RUN wget https://developer.arm.com/-/media/Files/downloads/gnu-rm/9-2020q2/gcc-arm-none-eabi-9-2020-q2-update-x86_64-linux.tar.bz2 | tar -xj
COPY gcc-arm-none-eabi-9-2020-q2-update-x86_64-linux.tar.bz2 /home/vscode/toolchain/gcc-arm-none-eabi-9-2020-q2-update-x86_64-linux.tar.bz2
RUN tar -xf /home/vscode/toolchain/gcc-arm-none-eabi-9-2020-q2-update-x86_64-linux.tar.bz2 --directory /home/vscode/toolchain

ENV PATH "/home/vscode/toolchain/gcc-arm-none-eabi-9-2020-q2-update/bin:$PATH"

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

# Create the user
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    #
    # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

# [Optional] Set the default user. Omit if you want to keep the default as root.
RUN chown -R $USERNAME /home/vscode
USER $USERNAME