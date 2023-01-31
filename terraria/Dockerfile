FROM ubuntu:latest

#INSTALL
RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y curl zip screen iproute2 sudo

#TERRARIA SERVER
EXPOSE 7777/tcp
RUN mkdir -p /opt/volume/data
VOLUME /opt/volume/data

RUN mkdir /opt/volume/log
VOLUME /opt/volume/log

RUN mkdir /opt/volume/config
VOLUME /opt/volume/config

RUN mkdir /root/terraria && cd /root/terraria
RUN mkfifo /opt/volume/log/log.txt

WORKDIR /root/terraria

RUN curl -L https://github.com/orelvis15/data/blob/master/v1-4-4-9.zip?raw=true -o terraria_server.zip && unzip terraria_server.zip
RUN ls
RUN mv Linux/* ../
RUN rm -r Windows Mac Linux

WORKDIR /root
RUN chmod +x TerrariaServer.bin.x86*

#create script init server
RUN echo "#!/bin/bash\n ./TerrariaServer.bin.x86_64 -config /opt/volume/config/config.txt > /opt/volume/log/log.txt" > init.sh
RUN chmod +x init.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "terraria", "./init.sh"]