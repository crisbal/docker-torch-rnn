FROM ubuntu:14.04
MAINTAINER Cristian Baldi "bld.cris.96@gmail.com"

ENV DEBIAN_FRONTEND noninteractive
ENV DEBCONF_NONINTERACTIVE_SEEN true

# Required packages
RUN apt-get update
RUN apt-get -y install \
    python \
    build-essential \
    python2.7-dev \
    python-pip \
    git \
    libhdf5-dev \
    software-properties-common

# Torch and luarocks
RUN git clone https://github.com/torch/distro.git /root/torch --recursive && cd /root/torch && \
    bash install-deps && \
    ./install.sh -b

ENV LUA_PATH='/root/.luarocks/share/lua/5.1/?.lua;/root/.luarocks/share/lua/5.1/?/init.lua;/root/torch/install/share/lua/5.1/?.lua;/root/torch/install/share/lua/5.1/?/init.lua;./?.lua;/root/torch/install/share/luajit-2.1.0-beta1/?.lua;/usr/local/share/lua/5.1/?.lua;/usr/local/share/lua/5.1/?/init.lua'
ENV LUA_CPATH='/root/.luarocks/lib/lua/5.1/?.so;/root/torch/install/lib/lua/5.1/?.so;./?.so;/usr/local/lib/lua/5.1/?.so;/usr/local/lib/lua/5.1/loadall.so'
ENV PATH=/root/torch/install/bin:$PATH
ENV LD_LIBRARY_PATH=/root/torch/install/lib:$LD_LIBRARY_PATH
ENV DYLD_LIBRARY_PATH=/root/torch/install/lib:$DYLD_LIBRARY_PATH
ENV LUA_CPATH='/root/torch/install/lib/?.so;'$LUA_CPATH

#torch-rnn and python requirements
WORKDIR /root
RUN git clone https://github.com/jcjohnson/torch-rnn && \
    pip install -r torch-rnn/requirements.txt

#Lua requirements
WORKDIR /root
RUN luarocks install torch
RUN luarocks install nn
RUN luarocks install optim
RUN luarocks install lua-cjson

RUN git clone https://github.com/deepmind/torch-hdf5 /root/torch-hdf5
WORKDIR /root/torch-hdf5
RUN luarocks make hdf5-0-0.rockspec

#Done!
WORKDIR /root/torch-rnn
