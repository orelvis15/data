FROM ubuntu:latest

RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y curl zip screen iproute2 sudo

RUN mkdir -p /opt/volume/data
VOLUME /opt/volume/data

RUN mkdir /opt/volume/log
VOLUME /opt/volume/log

RUN mkdir /root/gplay_server
WORKDIR /root/gplay_server
ADD config.json config.json

RUN curl -L {URL_FILE} -o server.zip && unzip server.zip

WORKDIR /root/gplay_server/bin/x64
RUN chmod +x factorio

RUN echo \#\!/bin/bash >> init.sh
RUN echo "./factorio --create /opt/volume/data/save.zip && ./factorio --start-server /opt/volume/data/save.zip --server-settings /root/gplay_server/config.json --console-log /opt/volume/log/log.txt" >> init.sh
RUN chmod +x init.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "gplaysection", "./init.sh"]