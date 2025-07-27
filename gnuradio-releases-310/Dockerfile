FROM ubuntu:24.04
LABEL maintainer="Federico 'Larroca' La rocca - flarroca@fing.edu.uy"

ENV DEBIAN_FRONTEND=noninteractive

RUN sed -i -e 's@http://.*.ubuntu.com@http://mirrors.tuna.tsinghua.edu.cn@g' -e 's@https://.*.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g' /etc/apt/sources.list

RUN apt-get update

# else it will output an error about Gtk namespace not found
RUN apt-get install -y gir1.2-gtk-3.0

# to have add-apt-repository available
RUN apt-get install -y software-properties-common

#RUN add-apt-repository -y ppa:gnuradio/gnuradio-releases

# create user gnuario with sudo (and password gnuradio)
RUN apt-get install -y sudo
RUN useradd --create-home --shell /bin/bash -G sudo gnuradio
RUN echo 'gnuradio:gnuradio' | chpasswd

# I create a dir at home which I'll use to persist after the container is closed (need to change it's ownership)
RUN mkdir /home/gnuradio/persistent  && chown gnuradio /home/gnuradio/persistent

RUN apt-get update

RUN apt-get install -y gnuradio

# installing other packages needed for downloading and installing OOT modules
RUN apt-get install -y gnuradio-dev cmake git libboost-all-dev libcppunit-dev liblog4cpp5-dev python3-pygccxml pybind11-dev liborc-dev python3-pip libgsl-dev clang-format

RUN apt-get install -y gr-osmosdr hackrf libhackrf-dev libhackrf0

# of course, nothing useful can be done without vim
RUN apt-get install -y vim zsh wget gedit

# chinese language
RUN apt-get install -y language-pack-zh-hans
RUN apt-get install -y fonts-droid-fallback ttf-wqy-zenhei ttf-wqy-microhei fonts-arphic-ukai fonts-arphic-uming

USER gnuradio

WORKDIR /home/gnuradio

# ohmyzsh
RUN sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"


ENV PYTHONPATH "${PYTHONPATH}:/usr/local/lib/python3/dist-packages"

CMD bash
