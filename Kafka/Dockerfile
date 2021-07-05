FROM ubuntu as base
EXPOSE 9092
ENV  KAFKA_HOME="/kafka"
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update
RUN apt-get install -y wget
RUN apt-get install -y --no-install-recommends tzdata
RUN apt-get install -y default-jdk


#x86
FROM base as amd64
RUN mkdir ~root/kafka
RUN wget -c https://ftp.cixug.es/apache/kafka/2.7.0/kafka_2.13-2.7.0.tgz -O kafka.tgz
RUN tar -zxvf kafka.tgz --directory ~root/kafka --strip-components=1 && rm kafka.tgz
RUN echo "dataDir=kafka/data/zookeeper" >> ~root/kafka/config/zookeeper.properties
RUN echo "listeners=PLAINTEXT://127.0.0.1:9092" >> ~root/kafka/config/server.properties
RUN echo "log.dirs=kafka/data/kafka" >> ~root/kafka/config/server.properties

# set path
RUN echo "export PATH="$PATH"" >> ~/.bashrc && . ~/.bashrc

CMD ~root/kafka/bin/zookeeper-server-start.sh ~root/kafka/config/zookeeper.properties && \
"/bin/sh -c '~root/kafka/bin/kafka-server-start.sh ~root/kafka/config/server.properties > ~root/kafka/kafka.log 2>&1'"

