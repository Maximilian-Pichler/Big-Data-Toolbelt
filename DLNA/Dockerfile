# maximilianpichler/samba:buildx-latest
FROM alpine as base
EXPOSE 1900/udp 8200/tcp

RUN apk add --no-cache minidlna\
    && rm -rf /var/cache/apk/*

RUN mkdir /NAS

# DLNA Configuration
RUN rm /etc/minidlna.conf
COPY minidlna.conf /etc/

CMD minidlnad -d -f /etc/minidlna.conf