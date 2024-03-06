## Make server accessible inside your local network    

Table of content:
- [Make server accessible inside your local network](#make-server-accessible-inside-your-local-network)
    - [Install samba to able to share/access files locally](#install-samba-to-able-to-shareaccess-files-locally)
    - [Now go to Windows in same network and map network drive](#now-go-to-windows-in-same-network-and-map-network-drive)


sudo apt install net-tools


sudo nano /etc/systemd/logind.conf
```
HandleLidSwitch=ignore
```
sudo service systemd-logind restart
sudo systemctl restart systemd-logind

#### Install samba to able to share/access files locally  

```bash
apt update 
sudo !! #this will run just above command with sudo
sudo apt install samba
samba --version

# make new shared dir
mkdir shared

sudo nano /etc/samba/smb.conf

```

``` conf
#Add following lines in end of smb.conf file 

[shared]
   comment = shared folder with samba
   path = /home/userver/shared
   read only = no
   browseable = yes
```

1. Then this ctrl+x to exit   
2. Y to save and enter to confirm 


```bash
# Restart the samba for update changes 
sudo service smbd restart

# Allow ubuntu firewall allow samba traffic 
sudo ufw allow samba

# Create password for network share 
sudo smbpasswd -a userver

```

#### Now go to Windows in same network and map network drive 
select following - <br>
1. drive letter – D:
2. \\192.168.1.111\shared
3. use user name and password to connect
	- userver
	- your_password


