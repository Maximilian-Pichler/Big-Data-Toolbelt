FROM ubuntu as base
ENV FLASK_APP=superset
RUN apt-get update \
    && apt-get install -y wget python3-pip libmysqlclient-dev

#x86
FROM base as amd64
RUN wget -c https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O anaconda.sh

#ARM
FROM base as arm64
RUN wget -c https://github.com/Archiconda/build-tools/releases/download/0.2.3/Archiconda3-0.2.3-Linux-aarch64.sh -O anaconda.sh


#BUILD
FROM ${TARGETARCH}
RUN bash anaconda.sh -b -p /root/anaconda
RUN touch /root/.bashrc
RUN echo /root/anaconda/etc/profile.d/conda.sh >> /root/.bashrc

RUN /root/anaconda/bin/conda update -y --all
RUN /root/anaconda/bin/conda create --name superset python=3.7.1 -y
RUN /root/anaconda/bin/conda install --name superset -c conda-forge pandas==1.2.4 pyarrow==3.0.0 -y
RUN yes | /root/anaconda/envs/superset/bin/pip install --upgrade pip
RUN /root/anaconda/envs/superset/bin/pip install setuptools mysqlclient PyJWT apache-superset

RUN /root/anaconda/envs/superset/bin/superset fab create-admin \
               --username admin \
               --firstname Superset \
               --lastname Admin \
               --email admin@superset.com \
               --password admin
RUN /root/anaconda/envs/superset/bin/superset db upgrade
RUN /root/anaconda/envs/superset/bin/superset init

CMD /root/anaconda/envs/superset/bin/superset run -h 0.0.0.0 -p 8088 --reload --debugger