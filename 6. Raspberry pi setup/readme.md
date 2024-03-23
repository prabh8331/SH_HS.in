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

sudo apt-get install openvswitch-switch

sudo netplan apply




# Docker installation 

https://docs.docker.com/engine/install/ubuntu/


- rsbry pi
ssh prabh@192.168.1.222
sudo chown -R prabh /opt/

- existing server userver
ssh userver@192.168.1.111
scp /opt/portainer/docker-compose.yaml prabh@192.168.1.222:/opt/portainer/

 mosquitto

scp /opt/mosquitto/docker-compose.yaml prabh@192.168.1.222:/opt/mosquitto/

scp /opt/mosquitto/config/mosquitto.conf prabh@192.168.1.222:/opt/mosquitto/config/


frigate 

scp /opt/frigate/docker-compose.yaml prabh@192.168.1.222:/opt/frigate/

scp /opt/frigate/config/config.yml prabh@192.168.1.222:/opt/frigate/config/


- rsbry pi
ssh prabh@192.168.1.222

cd /opt/portainer/
docker compose up -d


