FROM ubuntu as base_1
EXPOSE 8888
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
RUN mkdir ~root/projects
RUN bash anaconda.sh -b -p /root/anaconda
#RUN /root/anaconda/bin/conda update --override-channels -c main -c conda-forge -y --all
RUN /root/anaconda/bin/conda create --name development python=3.9 -y


FROM base_2 as amd64_2
RUN /root/anaconda/bin/conda install --name base --override-channels -c main -c conda-forge mamba

FROM base_2 as arm64_2
COPY Assets/mamba.tar.bz2 .
RUN /root/anaconda/bin/conda install --name base --offline mamba.tar.bz2 && rm mamba.tar.bz2 


FROM ${TARGETARCH}_2 as base_3
RUN /root/anaconda/bin/mamba install --name development --override-channels -c main -c conda-forge ipykernel jupyter jupyterlab jupyter-archive pandas matplotlib scikit-learn SQLAlchemy PyMySQL requests findspark -y
RUN /root/anaconda/envs/development/bin/python -m ipykernel install --name=development


# install spark
RUN mkdir ~root/spark
RUN wget -c https://ftp.cixug.es/apache/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz -O spark.tgz
RUN tar -xf spark.tgz --directory ~root/spark --strip-components=1 && rm spark.tgz
ENV SPARK_HOME=/root/spark

# set path
ENV PATH="${PATH}:/root/anaconda/bin:root/spark/bin"
RUN echo ". /root/anaconda/etc/profile.d/conda.sh" >> ~/.bashrc && . ~/.bashrc

# set entrypoint
CMD /root/anaconda/envs/development/bin/jupyter lab --ip=0.0.0.0 --ServerApp.token="" --notebook-dir=~root/projects/ --allow-root