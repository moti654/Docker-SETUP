FROM nvidia/cuda:12.4.1-devel-ubuntu22.04

RUN apt-get update
RUN ln -sf /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
RUN apt install -y --no-install-recommends wget
RUN apt install -y --no-install-recommends build-essential zlib1g-dev libncurses5-dev libgdbm-dev \
libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev libbz2-dev liblzma-dev vim libopencv-dev
RUN apt-get upgrade -y
RUN apt-get autoremove -y
RUN apt install -y python3-pip
RUN pip3 install pip==23.0.1
RUN pip3 install --upgrade setuptools
RUN pip3 install torch torchvision torchaudio
COPY requirements.txt ./
RUN pip3 install -r requirements.txt
