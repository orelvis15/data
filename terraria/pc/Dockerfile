FROM ubuntu:latest

RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y curl zip screen iproute2 sudo

RUN mkdir -p /opt/volume/data
VOLUME /opt/volume/data

RUN mkdir /opt/volume/log
VOLUME /opt/volume/log


RUN mkdir /root/gplay_server
WORKDIR /root/gplay_server
ADD config.txt config.txt


RUN curl -L {URL_FILE} -o server.zip && unzip server.zip
RUN ls
RUN mv Linux/* ../
RUN rm -r Windows Mac Linux

WORKDIR /root
RUN chmod +x TerrariaServer.bin.x86*

RUN echo \#\!/bin/bash >> init.sh
RUN echo "./TerrariaServer.bin.x86_64 -config /root/gplay_server/config.txt > /opt/volume/log/log.txt" > init.sh
RUN chmod +x init.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "gplaysection", "./init.sh"]