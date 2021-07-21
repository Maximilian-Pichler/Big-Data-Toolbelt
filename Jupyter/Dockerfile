FROM maximilianpichler/anaconda:buildx-latest as base_1
EXPOSE 8888
SHELL ["/bin/bash", "-c"]

# setup environment
RUN /root/anaconda/bin/conda create --name development python=3.9 -y
RUN /root/anaconda/bin/mamba install --name development --override-channels -c main -c conda-forge ipykernel jupyter jupyterlab jupyter-archive pandas matplotlib scikit-learn SQLAlchemy PyMySQL requests findspark -y
RUN /root/anaconda/envs/development/bin/python -m ipykernel install --name=development

# install spark
RUN mkdir ~root/spark
RUN curl -sSL https://ftp.cixug.es/apache/spark/spark-3.1.2/spark-3.1.2-bin-hadoop3.2.tgz > spark.tgz
RUN tar -xf spark.tgz --directory ~root/spark --strip-components=1 && rm spark.tgz
ENV SPARK_HOME=/root/spark

# set path
ENV PATH="${PATH}:root/spark/bin"

# set entrypoint
CMD /root/anaconda/envs/development/bin/jupyter lab --ip=0.0.0.0 --port=8888 --ServerApp.token="" --notebook-dir=~root/projects/ --allow-root