FROM ubuntu:latest

RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y curl zip screen iproute2 sudo

EXPOSE 7777/tcp

RUN mkdir -p /opt/volume/data
VOLUME /opt/volume/data

RUN mkdir /opt/volume/log
VOLUME /opt/volume/log

RUN mkdir /opt/volume/config
VOLUME /opt/volume/config

ADD config.txt /opt/volume/config.txt

RUN mkdir /root/gplay_server
RUN mkfifo /opt/volume/log/log.txt

WORKDIR /root/gplay_server

RUN curl -L {URL_FILE} -o server.zip && unzip server.zip
RUN ls
RUN mv Linux/* ../
RUN rm -r Windows Mac Linux

WORKDIR /root
RUN chmod +x TerrariaServer.bin.x86*

RUN echo "!/bin/bash\n mv /opt/volume/config.txt /opt/volume/config/config.txt && ./TerrariaServer.bin.x86_64 -config /opt/volume/config/config.txt > /opt/volume/log/log.txt" > init.sh
RUN chmod +x init.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "gplaysection", "./init.sh"]