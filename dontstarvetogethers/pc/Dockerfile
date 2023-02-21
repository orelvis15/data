FROM steamcmd/steamcmd:latest

# Instalacion de dependencias
# libcurl4-gnutls-dev es obligatoria para correr DontStarve
RUN apt-get -y update && apt-get -y upgrade
RUN apt-get install -y zip screen iproute2 sudo libcurl4-gnutls-dev

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
ADD gplaycluster gplaycluster

#Instalacion del servidor ID_STEAM:343050
RUN steamcmd +login anonymous +force_install_dir /root/gplay_server +app_update 343050 +quit

WORKDIR /root/gplay_server/bin64

RUN echo \#\!/bin/bash > init_master.sh
RUN echo \#\!/bin/bash > init_caves.sh

# Extraer la informacion del cluster 
RUN echo "cp -r ../gplaycluster /opt/volume/data && rm -r ../gplaycluster" >> init_master.sh

# Script para correr el servidor principal
# El parametro persistent_storage_root especifica la ruta de la configuracion
# El paramentro conf_dir especifica la carpeta de la configuracion
# Para este caso la ruta completa de la configuracion seria /opt/volume/data
RUN echo "./dontstarve_dedicated_server_nullrenderer_x64 -console -cluster gplaycluster -persistent_storage_root /opt/volume -conf_dir data -shard Master >> /opt/volume/log/log.txt" >> init_master.sh

#Script para correr las cavernas
RUN echo "./dontstarve_dedicated_server_nullrenderer_x64 -console -cluster gplaycluster -persistent_storage_root /opt/volume -conf_dir data -shard Caves >> /opt/volume/log/log.txt" >> init_caves.sh

RUN chmod +x init_master.sh
RUN chmod +x init_caves.sh

ENTRYPOINT ["screen", "-D", "-m", "-A", "-S", "gplaysection","./init_master.sh"]
