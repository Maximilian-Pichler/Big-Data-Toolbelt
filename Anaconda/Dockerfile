FROM ubuntu as base_1

# Install Packages
SHELL ["/bin/bash", "-c"]
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends tzdata curl python3-pip libmysqlclient-dev default-jdk zip unzip git

########################

# x86 64bit
FROM base_1 as amd64_1
RUN curl -sSL https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh > anaconda.sh

# ARM 64bit
FROM base_1 as arm64_1
RUN curl -sSL https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh > anaconda.sh

########################

# Install Anaconda
FROM ${TARGETARCH}_1 as base_2
RUN bash anaconda.sh -b -p /opt/anaconda \
    && /opt/anaconda/bin/conda update -y --all \
    && /opt/anaconda/bin/conda install -c conda-forge mamba -y
ENV PATH="${PATH}:/opt/anaconda/bin"
RUN touch ~/.bashrc \
    && echo ". /opt/anaconda/etc/profile.d/conda.sh" >> ~/.bashrc && . ~/.bashrc
