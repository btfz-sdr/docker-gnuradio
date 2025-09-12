FROM ubuntu:24.04
LABEL maintainer="Federico 'Larroca' La rocca - flarroca@fing.edu.uy"

ENV DEBIAN_FRONTEND=noninteractive

# 替换为阿里云Ubuntu镜像源（适用于各版本Ubuntu）
RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list && \
    sed -i s@/security.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list

RUN apt-get update

# 安装核心依赖和中文支持
RUN apt-get install -y gir1.2-gtk-3.0 software-properties-common sudo language-pack-zh-hans fonts-droid-fallback ttf-wqy-zenhei ttf-wqy-microhei fonts-arphic-ukai fonts-arphic-uming

# 生成中文UTF-8编码（需root权限，必须在切换用户前执行）
RUN locale-gen zh_CN.UTF-8

# 创建 gnuradio 用户
RUN useradd --create-home --shell /bin/bash -G sudo gnuradio
RUN echo 'gnuradio:gnuradio' | chpasswd

# 创建持久化目录并设置权限
RUN mkdir /home/gnuradio/persistent && chown gnuradio:gnuradio /home/gnuradio/persistent

# 安装其他软件包
RUN apt-get update
RUN apt-get install -y gnuradio gnuradio-dev cmake git libboost-all-dev libcppunit-dev liblog4cpp5-dev python3-pygccxml pybind11-dev liborc-dev python3-pip libgsl-dev clang-format gr-osmosdr hackrf libhackrf-dev libhackrf0 vim zsh wget gedit libuhd-dev uhd-host
RUN apt-get install -y alsa-utils pulseaudio-utils sox

# 切换到 gnuradio 用户
USER gnuradio

# 在 gnuradio 用户下进行语言环境设置
# 将 LANG 环境变量设置添加到 .bashrc 和 .zshrc
RUN echo "export LANG=zh_CN.UTF-8" >> /home/gnuradio/.bashrc
RUN echo "export LANGUAGE=zh_CN:zh" >> /home/gnuradio/.bashrc
RUN echo "export LC_ALL=zh_CN.UTF-8" >> /home/gnuradio/.bashrc

RUN echo "export LANG=zh_CN.UTF-8" >> /home/gnuradio/.zshrc
RUN echo "export LANGUAGE=zh_CN:zh" >> /home/gnuradio/.zshrc
RUN echo "export LC_ALL=zh_CN.UTF-8" >> /home/gnuradio/.zshrc

# 设置工作目录
WORKDIR /home/gnuradio

# 安装 oh-my-zsh (会用到 wget)
RUN sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# 设置 PYTHONPATH
ENV PYTHONPATH="/usr/local/lib/python3/dist-packages"

CMD ["zsh"]
