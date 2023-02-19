FROM ubuntu:latest

RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y curl zip screen iproute2 sudo

EXPOSE 34197/udp
RUN mkdir -p /opt/volume/data
VOLUME /opt/volume/data

RUN mkdir /opt/volume/log
VOLUME /opt/volume/log

RUN mkdir /opt/volume/config
VOLUME /opt/volume/config

ADD config.json /opt/volume/config.json

RUN mkdir /root/gplay_server
WORKDIR /root/gplay_server

RUN curl -L {URL_FILE} -o server.zip && unzip server.zip

WORKDIR /root/gplay_server/bin/x64
RUN chmod +x factorio

RUN echo "#!/bin/bash\n cp -fr /opt/volume/config.json /opt/volume/config/config.json && ./factorio --create /opt/volume/data/save.zip && ./factorio --start-server /opt/volume/data/save.zip --server-settings /opt/volume/config/config.json --console-log /opt/volume/log/log.txt" > init.sh
RUN chmod +x init.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "gplaysection", "./init.sh"]