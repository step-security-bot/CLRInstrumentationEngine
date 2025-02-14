# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License.

# This image is based on the public Ubuntu 18.04 LTS image
FROM mcr.microsoft.com/mirror/docker/library/ubuntu:18.04

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        software-properties-common

# Install .NET Core 2.1 SDK
# https://www.microsoft.com/net/download/linux-package-manager/ubuntu18-04/sdk-2.1.403
RUN curl -SL --output packages-microsoft-prod.deb https://packages.microsoft.com/config/ubuntu/18.04/packages-microsoft-prod.deb \
    && dpkg -i packages-microsoft-prod.deb \
    && rm packages-microsoft-prod.deb

RUN apt-get install -y --no-install-recommends apt-transport-https \
    && apt-get update \
    && apt-get install -y --no-install-recommends dotnet-sdk-2.1

# Install .NET 7 SDK
RUN curl -sSL https://dot.net/v1/dotnet-install.sh | bash /dev/stdin -c 7.0

# Trigger first run experience by running arbitrary cmd to populate local package cache
RUN dotnet help

RUN echo 'export DOTNET_ROOT=$HOME/.dotnet' >> ~/.bashrc
RUN echo 'export DOTNET_EXE=$DOTNET_ROOT/dotnet' >> ~/.bashrc
RUN echo 'export PATH=$DOTNET_ROOT:$DOTNET_ROOT/tools:$PATH' >> ~/.bashrc

# get the signing key for the kitware (cmake) repository
RUN curl https://apt.kitware.com/keys/kitware-archive-latest.asc 2>/dev/null | gpg --dearmor - | tee /etc/apt/trusted.gpg.d/kitware.gpg >/dev/null

# Add the repository (ubuntu 18.04) to get the newest version of cmake
# NOTE: the repository will need to be changed if we update the version of ubuntu
RUN apt-add-repository 'deb https://apt.kitware.com/ubuntu/ bionic main'

# CLR Instrumentation Engine Build Prerequisites
# CMake will be retrieved from the kitware repo
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        clang-3.9 \
        cmake \
        git \
        libxml2-dev \
        make \
        jq

# Git now checks for repository ownership
RUN git config --global --add safe.directory /root/ClrInstrumentationEngine/out/Linux/Intermediate/x64.Debug/_deps/googletest-src
RUN git config --global --add safe.directory /root/ClrInstrumentationEngine