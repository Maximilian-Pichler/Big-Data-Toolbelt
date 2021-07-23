# maximilianpichler/samba:buildx-latest
FROM alpine as base
EXPOSE 445 

RUN apk add --no-cache samba-common-tools samba-client samba-server \
    && rm -rf /var/cache/apk/*

RUN mkdir /NAS /NAS/media /NAS/documents

# Samba Configuration
RUN rm /etc/samba/smb.conf
COPY smb.conf /etc/samba/

CMD (echo root; echo root) | smbpasswd -a -s root\
    && smbd --foreground --log-stdout --no-process-group
