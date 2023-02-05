FROM frolvlad/alpine-glibc:alpine-3.12

#INSTALL
RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y curl zip screen iproute2 sudo xz-utils

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

RUN curl -L {URL_FILE} -o factorio_server.tar.xz && tar xf factorio_server.tar.xz

WORKDIR /root/factorio/bin/x64
RUN chmod +x factorio

#create script init server
RUN echo "#!/bin/bash\n ./factorio --create /opt/volume/data/initial.zip && ./factorio --start-server /opt/volume/data/initial.zip --server-settings /opt/volume/data/server-settings.example.json --console-log /opt/volume/log/log.txt" > init.sh
RUN chmod +x init.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "factorio", "./init.sh"]