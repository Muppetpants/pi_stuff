# pi_stuff

## Pi Set-up

Connect to WLAN via CLI (one-time) 

```bash

 # stop and disable NetworkManager (GUI) 
 sudo systemctl stop NetworkManager
 sudo systemctl disable NetworkManager
  
 # scan for nearby wlan network names (SSIDs)
 sudo iw dev <wireless_interface> scan | grep “SSID:”
 
 # create wlan conf for wpa_supplicant
 wpa_passphrase ‘<SSID>’ ‘<PASSWORD>’ > <ssid.conf>
 
 # disable previous wpa_supplicant processes 
 sudo killall -9 wpa_supplicant
 
 # connect to wlan via cli with wpa_passphrase ssid.conf
 sudo wpa_supplicant -c <path_to_ssid.conf> -i <wireless_interface> -B
 
 # request an ip address (as needed) 
 sudo dhclient <wireless_interface>
 
```

Connect to WLAN via CLI (persistent)  

```bash
# stop and disable NetworkManager (GUI) 
 sudo systemctl stop NetworkManager
 sudo systemctl disable NetworkManager
  
 # scan for nearby wlan network names (SSIDs)
 sudo iw dev <wireless_interface> scan | grep “SSID:”
 
 # create wpa_supplicant.conf for wpa_supplicant
 sudo su 
 wpa_passphrase ‘<SSID>’ ‘<PASSWORD>’ >> /etc/wpa_supplicant/wpa_supplicant.conf
 exit
 
 # disable previous wpa_supplicant processes 
 sudo killall -9 wpa_supplicant
 
 # connect to wlan via cli with wpa_passphrase ssid.conf
 sudo wpa_supplicant -c <path_to_ssid.conf> -i <wireless_interface> -B
 
 # request an ip address (as needed) 
 sudo dhclient <wireless_interface>
 
```

Remote administration (SSH Server)

```bash
# turn on SSH server (on remote host) 
 sudo systemctl start ssh
 
 # enable SSH server (on remote host) to start at reboot
 sudo systemctl enable ssh
 
 # restart SSH server (on remote host) after .conf changes
 sudo systemctl start ssh
 
 # confirm ssh is running (on remote host) on a particular port 
 netstat -pant 
```

Remote administration (SSH Client) 

```bash
# ssh into a remote host
ssh -p <port_number> <user_name>@<ip_address> 

# scp pull 
scp -P <port_number> <user_name>@<ip_address>:<path_to_source> <path_to_destination>
 
# scp push 
scp <path_to_source> -P <port_number> <user_name>@<ip_address>:<path_to_destination>

# create ssh key pair (on local machine) 
ssh-keygen -t <algorithm> -b <size>
#e.g. ssh-keygen -t rsa -b 4096

# ssh copy public key to remote host  
ssh-copy-id -i <path_to_public_key> -p <port_number> <user_name>@<ip_address>
```

FTP client 

```bash

# ftp access
ftp <ip_address> <port>
> Enter Username:<username>
> Enter Password: <password>

# ftp commands
ls # files in current directory 
cd <directory> # change directory to <directory>
get <file> # download <file> to local pwd
mget <files> # download <files> to local pwd

```

Pi Camera

```bash
# enable legacy camera support via sudo raspi-config
sudo apt update 
sudo apt --no-install-recommends install -y ca-certificates curl python3 python3-dev libcurl4-openssl-dev gcc libssl-dev
sudo python3 -m pip install --pre motioneye
sudo motioneye_init
sudo systemctl disable motion.service
sudo systemctl stop motion.service
# in browser, visit 127.0.0.1:8765, login:admin password:blank
# in browser, add camera (defaults)
sudo sed -i 's/video10/video0/g' /etc/motioneye/camera-1.conf
sudo systemctl restart motioneye.service
# browse to 127.0.0.1:8765, login:admin password:blank
```
