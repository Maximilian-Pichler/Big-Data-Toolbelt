FROM ubuntu as base_1
SHELL ["/bin/bash", "-c"]

# Install Packages
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get upgrade -y \
RUN apt-get install -y --no-install-recommends tzdata curl python3-pip libmysqlclient-dev default-jdk zip unzip git

# x86 64bit
FROM base_1 as amd64_1
RUN curl -sSL https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh > anaconda.sh

# ARM 32bit
FROM base_1 as arm_1
RUN curl -sSL https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-armv7l.sh > anaconda.sh

# ARM 64bit
FROM base_1 as arm64_1
RUN curl -sSL https://github.com/Archiconda/build-tools/releases/download/0.2.3/Archiconda3-0.2.3-Linux-aarch64.sh > anaconda.sh

# Main Build
FROM ${TARGETARCH}_1 as base_2

# Install Anaconda
RUN bash anaconda.sh -b -p /root/anaconda
RUN /root/anaconda/bin/conda update -y --all
#RUN /root/anaconda/bin/conda install -c conda-forge mamba -y # not available for arm 32bit 

# Set Path
ENV PATH="${PATH}:/root/anaconda/bin"
RUN touch ~/.bashrc
RUN echo ". /root/anaconda/etc/profile.d/conda.sh" >> ~/.bashrc && . ~/.bashrc
