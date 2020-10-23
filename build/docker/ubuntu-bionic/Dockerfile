# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

#
# Apache Thrift Docker build environment for Ubuntu Bionic
# with some updated packages.
#

FROM buildpack-deps:bionic-scm
MAINTAINER Apache Thrift <dev@thrift.apache.org>
ENV DEBIAN_FRONTEND noninteractive

### Add apt repos

RUN apt-get update && \
    apt-get dist-upgrade -y && \
    apt-get install -y --no-install-recommends --fix-missing \
      apt \
      apt-transport-https \
      apt-utils \
      curl \
      dirmngr \
      software-properties-common \
      wget

# Dart
RUN curl https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
    curl https://storage.googleapis.com/download.dartlang.org/linux/debian/dart_stable.list > \
      /etc/apt/sources.list.d/dart_stable.list

# dotnet (netcore)
RUN curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/microsoft.gpg && \
    wget -q -O /etc/apt/sources.list.d/microsoft-prod.list https://packages.microsoft.com/config/ubuntu/18.04/prod.list && \
    chown root:root /etc/apt/trusted.gpg.d/microsoft.gpg && \
    chown root:root /etc/apt/sources.list.d/microsoft-prod.list

# erlang
RUN wget -O- https://packages.erlang-solutions.com/ubuntu/erlang_solutions.asc | apt-key add - && \
    echo "deb https://packages.erlang-solutions.com/ubuntu bionic contrib" | tee /etc/apt/sources.list.d/erlang.list

# node.js
RUN curl -sL https://deb.nodesource.com/gpgkey/nodesource.gpg.key | apt-key add - && \
    echo "deb https://deb.nodesource.com/node_10.x bionic main" | tee /etc/apt/sources.list.d/nodesource.list

### install general dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
`# General dependencies` \
      bash-completion \
      bison \
      build-essential \
      clang \
      cmake \
      debhelper \
      flex \
      gdb \
      libasound2 \
      libatk-bridge2.0-0 \
      libgtk-3-0 \
      llvm \
      ninja-build \
      pkg-config \
      unzip \
      valgrind \
      vim
ENV PATH /usr/lib/llvm-6.0/bin:$PATH

# lib/as3 (ActionScript)
RUN mkdir -p /usr/local/adobe/flex/4.6 && \
    cd /usr/local/adobe/flex/4.6 && \
    wget -q "http://download.macromedia.com/pub/flex/sdk/flex_sdk_4.6.zip" && \
    unzip flex_sdk_4.6.zip
ENV FLEX_HOME /usr/local/adobe/flex/4.6

RUN apt-get install -y --no-install-recommends \
`# C++ dependencies` \
      libboost-all-dev \
      libevent-dev \
      libssl-dev \
      qt5-default \
      qtbase5-dev \
      qtbase5-dev-tools

ENV SBCL_VERSION 2.0.9
RUN \
`# Common Lisp (sbcl) dependencies` \
    curl --version && \
    curl -o sbcl-${SBCL_VERSION}-arm64-linux-binary.tar.bz2 -J -L https://sourceforge.net/projects/sbcl/files/sbcl/${SBCL_VERSION}/sbcl-${SBCL_VERSION}-arm64-linux-binary.tar.bz2/download?use_mirror=managedway# && \
    ls
    #tar xjf sbcl-${SBCL_VERSION}-arm64-linux-binary.tar.bz2 \
    #cd sbcl-${SBCL_VERSION}-arm64-linux && \
    #./install.sh && \
    #sbcl --version && \
    #cd .. && \
    #rm -rf sbcl*
