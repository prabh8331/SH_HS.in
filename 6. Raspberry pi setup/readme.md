1. downlaod raspberry pi imager from offical website and install

sudo apt update

sudo apt install net-tools

ifconfig

note ip address and ssh to the respberry pi

make static ip address

sudo nano /etc/netplan/50-cloud-init.yaml

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: false
      addresses: [192.168.1.222/24]
      routes:
        - to: 0.0.0.0/0
          via: 192.168.1.1
          on-link: true
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]

```

```
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: false
      addresses: [192.168.1.222/24]
      routes:
        - to: 0.0.0.0/0
          via: 192.168.1.1
          on-link: true
      nameservers:
        addresses: [8.8.8.8, 8.8.4.4]
  wifis:
    renderer: networkd
    wlan0:
      access-points:
        TP-Link_PS:
          password: b48353e3e8fa44c2dfd00c186972eb214a4eedac76ebb92cb63670821eb94250
      dhcp4: true
      optional: true



```


sudo apt-get install openvswitch-switch

sudo netplan apply




# Docker installation 

https://docs.docker.com/engine/install/ubuntu/


- rsbry pi
ssh prabh@192.168.1.222
sudo chown -R prabh /opt/

cd /opt/
mkdir portainer

- existing server userver
ssh userver@192.168.1.111
scp /opt/portainer/docker-compose.yaml prabh@192.168.1.222:/opt/portainer/

<!-- in case of server change  -->
ssh-keygen -f "/home/userver/.ssh/known_hosts" -R "192.168.1.222"


docker ps --filter "name=portainer"



 mosquitto

scp /opt/mosquitto/docker-compose.yaml prabh@192.168.1.222:/opt/mosquitto/

scp /opt/mosquitto/config/mosquitto.conf prabh@192.168.1.222:/opt/mosquitto/config/


frigate 

sudo nano /boot/config.txt

gpu_mem=1024

sudo reboot

cd /opt/
mkdir frigate
cd frigate/

sudo nano docker-compose.yaml
docker-compose.yaml
```yml
version: "3.9"
services:
  frigate:
    container_name: frigate
    restart: never
    image: ghcr.io/blakeblackshear/frigate:stable
    shm_size: "64mb"
    devices:
      - /dev/video11:/dev/video11
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /opt/frigate/config/config.yml:/config/config.yml:ro
      - /opt/frigate/media:/media/frigate
      - type: tmpfs # Optional: 1GB of memory, reduces SSD/SD Card wear
        target: /tmp/cache
        tmpfs:
          size: 1000000000
    ports:
      - "5000:5000"
      - "8554:8554" # RTMP feeds
    environment:
      FRIGATE_RTSP_PASSWORD: "password"

```

cd config
sudo nano config.yml

```yml
mqtt:
  enabled: False

cameras:
  Camgate:
    ffmpeg:
      inputs:
        - path: rtsp://admin:yoyo@8331yo@192.168.1.64
          roles:
            - detect
            - rtmp
            - record
    detect:
      width: 1280
      height: 720
      fps: 24

    record:
      enabled: true
      retain:
        days: 15
        mode: motion
      events:
        retain:
          default: 30
          mode: motion

    snapshots:
      enabled: True
      retain:
        default: 30


  Camgate2:
    ffmpeg:
      inputs:
        - path: rtsp://admin:yoyo@8331yo@192.168.1.65
          roles:
            - detect
            - rtmp
            - record
    detect:
      width: 1280
      height: 720
      fps: 24



    record:
      enabled: True


```


- rsbry pi
ssh prabh@192.168.1.222

cd /opt/portainer/
docker compose up -d


