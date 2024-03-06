#### Install docker 


Docker allows you to package and run applications in containers

Containers are lightweight, standalone, and portable environments that package an application and its dependencies


Portainer is a user-friendly web-based management interface for Docker

Providing an intuitive graphical interface to easily create, manage, and monitor containers, images, volumes, and networks


[Official_ubuntu_docker_setup](https://docs.docker.com/engine/install/ubuntu/)

https://www.geeksforgeeks.org/introduction-to-docker/

```bash

sudo docker ps 
# list of all the docker containers 
sudo docker container ls -a

# stop any docker container 
sudo docker stop 9c41f96f6f14

# remove any container
sudo docker container rm c2de0b52bf31

# see the logs of any docker container 
sudo docker logs 049ff5c844e6

#
sudo docker images
```


```bash
# add to normal user

grep docker /etc/group
sudo usermod -aG docker userver
newgrp docker

docker ps


```


ls -l

add premisionss to a user for read and write access to a folder 
---- this command change premissions chmod +rwx /opt/plex/media

--- this command change user & worked 

sudo chown -R prabh:prabh /opt/plex/media


-- check the list of ports 
sudo ss -tuln



```bash
##portainer setup docker-compose.yaml
use /opt folder for this
and also use this folder for volumes 

sudo chown -R userver:userver /opt

cd /opt
ls
mkdir portainer
nano docker-compose.yaml

copy paste the following command in file

version: '3.0'

services:
  portainer:
    container_name: portainer
    image: portainer/portainer-ce
    restart: always
    ports:
      - "9000:9000/tcp"
    environment:
      - TZ=Asia/Kolkata
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - /opt/portainer:/data

sudo docker compose up -d

sudo docker container ls

http://192.168.1.111:9000/


```



sudo docker run -d --restart always cloudflare/cloudflared:latest tunnel --no-autoupdate run --token XXXX--XXXX



