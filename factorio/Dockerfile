FROM ubuntu:latest

#INSTALL
RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y curl zip screen iproute2 sudo

#TERRARIA SERVER
EXPOSE 34197/udp
RUN mkdir -p /opt/volume/data
VOLUME /opt/volume/data

RUN mkdir /opt/volume/log
VOLUME /opt/volume/log

RUN mkdir /opt/volume/config
VOLUME /opt/volume/config

RUN mkdir /root/factorio && cd /root/factorio

WORKDIR /root/factorio

RUN curl -L {URL_FILE} -o factorio_server.zip && unzip factorio_server.zip

WORKDIR /root/factorio/bin/x64
RUN chmod +x factorio

#create script init server
RUN echo "#!/bin/bash\n ./factorio --create /opt/volume/data/save.zip && ./factorio --start-server /opt/volume/data/save.zip --server-settings /opt/volume/config/config.json --console-log /opt/volume/log/log.txt" > init.sh
RUN chmod +x init.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "factorio", "./init.sh"]