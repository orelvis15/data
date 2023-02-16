FROM ubuntu:latest

RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y curl screen iproute2 sudo openjdk-17-jre

EXPOSE 25565/tcp

RUN mkdir -p /opt/volume/data
VOLUME /opt/volume/data

RUN mkdir /opt/volume/log
VOLUME /opt/volume/log

RUN mkdir /opt/volume/config
VOLUME /opt/volume/config



RUN mkdir /root/gplay_server
RUN mkfifo /opt/volume/log/log.txt

WORKDIR /root/gplay_server
ADD eula.txt
ADD config.txt /opt/volume/server.properties

RUN curl -L https://launcher.mojang.com/v1/objects/c8f83c5655308435b3dcf03c06d9fe8740a77469/server.jar -o server.jar

RUN echo "!/bin/bash\n java -Xms1024M -Xmx1024M -jar server.jar nogui --world /opt/volume/data" > init.sh
RUN chmod +x init.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "gplaysection", "./init.sh"]