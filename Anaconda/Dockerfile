FROM ubuntu as base_1
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update
RUN apt-get install -y --no-install-recommends tzdata
RUN apt-get install -y wget python3-pip libmysqlclient-dev default-jdk
RUN apt-get install -y zip unzip git

#x86
FROM base_1 as amd64_1
RUN wget -c https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O anaconda.sh

#ARM
FROM base_1 as arm64_1
RUN wget -c https://github.com/Archiconda/build-tools/releases/download/0.2.3/Archiconda3-0.2.3-Linux-aarch64.sh -O anaconda.sh


#BUILD
FROM ${TARGETARCH}_1 as base_2

# install anaconda
RUN bash anaconda.sh -b -p /root/anaconda
RUN /root/anaconda/bin/conda update -y --all
RUN /root/anaconda/bin/conda install -c conda-forge mamba -y

# set path
ENV PATH="${PATH}:/root/anaconda/bin"
RUN echo ". /root/anaconda/etc/profile.d/conda.sh" >> ~/.bashrc && . ~/.bashrc
