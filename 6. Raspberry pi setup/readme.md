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
    restart: always
    privileged: true
    image: ghcr.io/blakeblackshear/frigate:stable
    shm_size: "64mb"
    devices:
      - /dev/video11:/dev/video11
    volumes:
      - /etc/localtime:/etc/localtime:ro
      - /opt/frigate/config/config.yml:/config/config.yml:ro
      - /mnt/sdcard:/media/frigate
      #- /opt/frigate/media:/media/frigate
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

mkdir config
cd config
sudo nano config.yml

```yml
mqtt:
  enabled: False

cameras:
  Camgate:
    ffmpeg:
      hwaccel_args: preset-rpi-64-h264
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

Raspbarry pi hardware accleration

1. Increading gpu memory 

sudo apt-get update

sudo apt-get install raspi-config

sudo raspi-config

Performance Options" > "GPU Memory" and set it to at least 128MB.
set to 256

vcgencmd get_mem gpu


2. Overclock 

sudo apt update && sudo apt upgrade -y
sudo apt-get install sysbench

sudo apt update
sudo apt dist-upgrade

vcgencmd measure_clock arm

sysbench --test=cpu --cpu-max-prime=2000 --num-threads=4 run

```
prabh@ubuntu:~$ sysbench --test=cpu --cpu-max-prime=2000 --num-threads=4 run
WARNING: the --test option is deprecated. You can pass a script name or path on the command ine without any options.
WARNING: --num-threads is deprecated, use --threads instead
sysbench 1.0.20 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 4
Initializing random number generator from current time


Prime numbers limit: 2000

Initializing worker threads...

Threads started!

CPU speed:
    events per second: 38820.01

General statistics:
    total time:                          10.0007s
    total number of events:              388335

Latency (ms):
         min:                                    0.07
         avg:                                    0.10
         max:                                   29.45
         95th percentile:                        0.07
         sum:                                39821.60

Threads fairness:
    events (avg/stddev):           97083.7500/2315.29
    execution time (avg/stddev):   9.9554/0.01


```

sudo cp /boot/config.txt /boot/config.txt.bak

sudo nano /boot/config.txt

arm_freq=2100
gpu_freq=750
over_voltage=6



sudo reboot


sysbench --test=cpu --cpu-max-prime=2000 --num-threads=4 run


```

prabh@ubuntu:~$ sysbench --test=cpu --cpu-max-prime=2000 --num-threads=4 run
WARNING: the --test option is deprecated. You can pass a script name or path on              the command line without any options.
WARNING: --num-threads is deprecated, use --threads instead
sysbench 1.0.20 (using system LuaJIT 2.1.0-beta3)

Running the test with following options:
Number of threads: 4
Initializing random number generator from current time


Prime numbers limit: 2000

Initializing worker threads...

Threads started!

CPU speed:
    events per second: 43825.16

General statistics:
    total time:                          10.0004s
    total number of events:              438392

Latency (ms):
         min:                                    0.07
         avg:                                    0.09
         max:                                   64.17
         95th percentile:                        0.07
         sum:                                39825.15

Threads fairness:
    events (avg/stddev):           109598.0000/6640.52
    execution time (avg/stddev):   9.9563/0.00



```



- rsbry pi
ssh prabh@192.168.1.222

cd /opt/portainer/
docker compose up -d



hkvision camera paramater change to make video quality little to so that recoding is better

current - 

stream type - main stream (Normal)
resolution - 1920*1080P
bitreate type - variable 
video quality - - medium 
frame rate- 25 fps
mzx bitrate - - 4096
video endcoing - h.264
h.264+ - off
profile - main profile 
i frame interval - 50
smmothing - 50

after changes - 

![Alt text](<Screenshot 2024-03-24 123057.png>)



techniques applied to impove the encoding performance - 

Reduce Camera Resolution: Lower the resolution of the camera feed in Frigate's configuration. This can reduce the amount of data that needs to be processed and improve performance.

Adjust FPS (Frames Per Second): Reduce the FPS setting in the camera configuration to lower the processing load on the Raspberry Pi.

Overclock the Raspberry Pi: If you're comfortable with it, you can try overclocking your Raspberry Pi. This can increase its performance, but be aware that it may void the warranty and could potentially cause stability issues if not done correctly.

assign More RAM to gpu: increase gpu ram




## boot using the usb drive 

after boot from sd open terminal and 

sudo apt-get upgrade -y


sudo rpi-eeprom-update

sudo rpi-eeprom-update -a

make a new bootable usb drive using raspberry pi imager

follow all previous steps: neetd to update the sudo nano /etc/netplan/50-cloud-init.yaml to connect with ethernet cable


sudo apt update
sudo apt full-upgrade

sudo apt install rpi-eeprom
sudo rpi-eeprom-update -d -a

<!-- echo program_usb_boot_mode=1 | sudo tee -a /boot/config.txt

sudo nano /boot/config.txt

sudo reboot

sudo rpi-eeprom-config --edit

add in the end of file 
BOOT_ORDER=0xf41


vcgencmd otp_dump | grep 17:
if output is - 17:3020000a , then usb boot mood is enabled
 -->





use sd card as storage in ressbary pi
1. format the sd card 
2. insert the sd card (and usb both)

sudo apt update
sudo apt install exfat-fuse

<!-- insted of inserting in sd slot insert with card reader, because it alway boot first from sd card -->

<!-- everywere mmcblk0 will be changed -->

sudo mkfs.exfat /dev/mmcblk0


sudo fdisk /dev/mmcblk0
Press n to create a new partition.
Choose the default options to create a primary partition using the entire disk.
Press w to write the changes and exit.

sudo mkfs.fat -F32 /dev/mmcblk0p1


<!-- sudo apt update
sudo apt install exfat-fuse exfat-utils -->



sudo chown -R prabh /opt/
cd /opt/
mkdir sdcard

lsblk
- sd name is - mmcblk0

sudo mount -t exfat /dev/mmcblk0 /opt/sdcard

sudo mkdir /mnt/sdcard
<!-- sudo mount /dev/mmcblk0p1 /mnt/sdcard -->
sudo mount /dev/sdb1 /mnt/sdcard



verify - 
<!-- ls /opt/sdcard
df -h /opt/sdcard -->
ls /mnt/sdcard
df -h /mnt/sdcard


auttomatically mount the sd card wihle system boot

sudo blkid /dev/sdb1
/dev/sdb1: UUID="2320-D92C" BLOCK_SIZE="512" TYPE="vfat" PARTUUID="f6df04ed-01"

sudo nano /etc/fstab

add in the end - 
<!-- /dev/mmcblk0p1 /mnt/sdcard           auto    defaults          0       2 -->
UUID=2320-D92C /mnt/sdcard  vfat  defaults  0  2


sudo mount -a
sudo reboot




docker run -d --restart always cloudflare/cloudflared:latest tunnel --no-autoupdate run --token