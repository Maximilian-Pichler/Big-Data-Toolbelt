FROM maximilianpichler/bdtb_anaconda:buildx-latest as base_1
EXPOSE 8888
SHELL ["/bin/bash", "-c"]

# setup environment
# x86 64bit
FROM base_1 as amd64_1
RUN /opt/anaconda/bin/conda create --name development python=3.9 -y

# ARM 64bit
FROM base_1 as arm64_1
RUN /opt/anaconda/bin/conda create --name development python=3.9 -y


FROM ${TARGETARCH}_1 as base_2
RUN /opt/anaconda/bin/mamba install --name development --override-channels -c main -c conda-forge ipykernel jupyter jupyterlab jupyter-archive pandas matplotlib scikit-learn SQLAlchemy PyMySQL requests findspark -y
RUN /opt/anaconda/envs/development/bin/python -m ipykernel install --name=development

# install spark
RUN mkdir /opt/spark
RUN curl -sSL https://ftp.cixug.es/apache/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz > spark.tgz
RUN tar -xf spark.tgz --directory /opt/spark --strip-components=1 && rm spark.tgz
ENV SPARK_HOME=/opt/spark

# set path
ENV PATH="${PATH}:root/spark/bin"

# set entrypoint
#--ip=0.0.0.0 
CMD /opt/anaconda/envs/development/bin/jupyter lab --port=8888 --ServerApp.token="" --allow-root
