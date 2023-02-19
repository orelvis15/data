FROM steamcmd/steamcmd:latest

# Instalacion de dependencias
# libcurl4-gnutls-dev es obligatoria para correr DontStarve
RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y zip screen iproute2 sudo libcurl4-gnutls-dev

EXPOSE 10999/tcp
EXPOSE 10999/udp

# Creacion de volumenes
RUN mkdir -p /opt/volume/data
VOLUME /opt/volume/data

RUN mkdir /opt/volume/log
VOLUME /opt/volume/log

# Creacion de la carpeta gplay_server
RUN mkdir /root/gplay_server
WORKDIR /root/gplay_server

# Copia de la informacion del cluster
# DontStarve guarda los datos en la misma carpeta de la configuracion,
# Crea subcarpetas save dentro de Master y Caves en las que guarda el estado del mapa
ADD gplayCluster.zip gplayCluster.zip

#Instalacion del servidor
RUN steamcmd +login anonymous +force_install_dir /root/gplay_server +app_update 343050 +quit

WORKDIR /root/gplay_server/bin64

RUN echo \#\!/bin/bash > init.sh

# Extraer la informacion del cluster 
RUN echo "unzip /root/gplay_server/gplayCluster.zip -d /opt/volume/data && rm /root/gplay_server/gplayCluster.zip" >> init.sh

# Correr el ejecutable
# El parametro persistent_storage_root especifica la ruta de la configuracion
# El paramentro conf_dir especifica la carpeta de la configuracion
# Para este caso la ruta completa de la configuracion seria /opt/volume/data
RUN echo "./dontstarve_dedicated_server_nullrenderer_x64 -console -cluster gplayCluster -persistent_storage_root /opt/volume/ -conf_dir data > /opt/volume/log/log.txt" >> init.sh

RUN chmod +x init.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "gplaysection", "./init.sh"]