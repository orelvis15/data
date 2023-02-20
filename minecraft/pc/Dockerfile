FROM ubuntu:latest

RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y curl screen zip iproute2 sudo openjdk-17-jre

RUN mkdir -p /opt/volume/data
VOLUME /opt/volume/data

RUN mkdir /opt/volume/log
VOLUME /opt/volume/log

RUN mkdir /root/gplay_server
WORKDIR /root/gplay_server
ADD server.properties server.properties

RUN curl -L https://raw.githubusercontent.com/orelvis15/data/master/minecraft/pc/versions/1-19-3.zip?raw=true -o server.zip && unzip server.zip

RUN echo "eula=true" > eula.txt

RUN echo "!/bin/bash\n rm /opt/volume/log/log.txt && java -jar server.jar nogui --world /opt/volume/data >> /opt/volume/log/log.txt" > init.sh

RUN chmod +x init.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "gplaysection", "./init.sh"]