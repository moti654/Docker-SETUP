FROM nvidia/cuda:12.2.0-devel-ubuntu20.04

RUN apt-get update
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
RUN apt install -y --no-install-recommends wget
RUN apt install -y --no-install-recommends build-essential zlib1g-dev libncurses5-dev libgdbm-dev \
libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev libbz2-dev liblzma-dev vim libopencv-dev
RUN wget --no-check-certificate https://www.python.org/ftp/python/3.10.13/Python-3.10.13.tgz \
&& tar -xf Python-3.10.13.tgz \
&& cd Python-3.10.13 \
&& ./configure --enable-optimizations\
&& make \
&& make install
RUN apt-get autoremove -y
RUN pip3 install pip==23.0.1
RUN pip3 install --upgrade setuptools
RUN pip3 install torch==2.0.1+cu118 torchvision==0.15.2+cu118 torchaudio==2.0.2 --index-url https://download.pytorch.org/whl/cu118
COPY requirements.txt ./
RUN pip3 install -r requirements.txt
