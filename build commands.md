```sh
sudo docker buildx build --push --platform linux/arm64/v8,linux/amd64 --tag maximilianpichler/bdtb_superset:buildx-latest .

sudo docker buildx build --push --platform linux/arm64/v8 --tag maximilianpichler/test:arm .

```

# Final
## Superset
sudo docker buildx build --push --platform linux/arm64,linux/arm,linux/amd64 --tag maximilianpichler/bdtb_superset:buildx-latest .

sudo docker run -d -p 8088:8088 --name superset maximilianpichler/bdtb_superset:buildx-latest

## Anaconda
sudo docker buildx build --push --platform linux/arm64,linux/amd64 --tag maximilianpichler/bdtb_anaconda:buildx-latest .

## Jupyter
sudo docker buildx build --push --platform linux/arm64,linux/arm,linux/amd64 --tag maximilianpichler/bdtb_jupyter:buildx-latest .

sudo docker run -d \
-p 8888:8888 \
--name jupyter \
--mount type=bind,source="$(pwd)",target=/root/projects/ \
--restart=unless-stopped
maximilianpichler/bdtb_jupyter:buildx-latest

## Kafka
sudo docker buildx build --push --platform linux/arm64,linux/arm,linux/amd64 --tag maximilianpichler/bdtb_kafka:buildx-latest .

sudo docker run -d \
-p 9092:9092 \
--name kafka \
--restart=unless-stopped
maximilianpichler/bdtb_kafka:buildx-latest 

## Samba
sudo docker buildx build --push --platform linux/arm64,linux/arm,linux/amd64 --tag maximilianpichler/samba:buildx-latest .

export SHARE_FOLDER=~/data/NAS \
&& sudo docker run -d \
-p 445:445 \
--name samba \
--mount type=bind,source="$SHARE_FOLDER",target=/NAS \
--restart=unless-stopped \
maximilianpichler/samba:buildx-latest

## DLNA
sudo docker buildx build --push --platform linux/arm64,linux/arm,linux/amd64 --tag maximilianpichler/minidlna:buildx-latest .

export SHARE_FOLDER=~/data/NAS \
&& sudo docker run -d \
--net="host" \
--name dlna \
--mount type=bind,source="$SHARE_FOLDER",target=/NAS \
--restart=unless-stopped \
maximilianpichler/minidlna:buildx-latest

#-p 1900:1900/udp \
#-p 8200:8200/tcp \


# Interactive View
## Superset
sudo docker run -it maximilianpichler/bdtb_superset:buildx-latest bash 

## Anaconda
sudo docker run --name anaconda -it maximilianpichler/bdtb_anaconda:buildx-latest bash

## Jupyter
sudo docker run -p 8888:8888 --name jupyter -it maximilianpichler/bdtb_jupyter:buildx-latest bash

## Kafka
sudo docker run -p 9092:9092 --name kafka -it maximilianpichler/bdtb_kafka:buildx-latest bash

## Samba
sudo docker run -p 445:445 --name samba -it maximilianpichler/samba:buildx-latest sh