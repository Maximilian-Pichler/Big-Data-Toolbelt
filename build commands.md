```sh
sudo docker buildx build --push --platform linux/arm64/v8,linux/amd64 --tag maximilianpichler/bdtb_superset:buildx-latest .

sudo docker buildx build --push --platform linux/arm64/v8 --tag maximilianpichler/test:arm .

```

# Final
## Superset
sudo docker buildx build --push --platform linux/arm64,linux/arm,linux/amd64 --tag maximilianpichler/bdtb_superset:buildx-latest .

sudo docker run -d -p 8088:8088 --name superset maximilianpichler/bdtb_superset:buildx-latest

## Anaconda
sudo docker buildx build --push --platform linux/arm64,linux/arm,linux/amd64 --tag maximilianpichler/anaconda:buildx-latest .

## Jupyter
sudo docker buildx build --push --platform linux/arm64,linux/arm,linux/amd64 --tag maximilianpichler/bdtb_jupyter:buildx-latest .

sudo docker run -d \
-p 8888:8888 \
--name jupyter \
--mount type=bind,source="$(pwd)"/../Storage,target=/root/projects/ \
maximilianpichler/bdtb_jupyter:buildx-latest

## Kafka
sudo docker buildx build --push --platform linux/arm64,linux/arm,linux/amd64 --tag maximilianpichler/bdtb_kafka:buildx-latest .

sudo docker run -d \
-p 9092:9092 \
--name kafka \
maximilianpichler/bdtb_kafka:buildx-latest 

# Interactive View
## Superset
sudo docker run -it maximilianpichler/bdtb_superset:buildx-latest bash 

## Jupyter
sudo docker run -p 8888:8888 --name jupyter -it maximilianpichler/bdtb_jupyter:buildx-latest bash

## Kafka
sudo docker run -p 9092:9092 --name kafka -it maximilianpichler/bdtb_kafka:buildx-latest bash