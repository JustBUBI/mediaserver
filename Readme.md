If on VM, find the IP to connect via SSH -> ip a

sudo apt update && sudo apt upgrade -y

Install Docker:
sudo apt install -y docker.io docker-compose
sudo usermod -aG docker $USER
newgrp docker

Create the following folder structure:
mkdir app app/{config,secrets} media media/{movies,series} downloads -v
mkdir /host/data /host/data/media /host/data/media/{movies,tv} /host/data/torrents /host/data/torrents/{movies,tv} -v
sudo chown -R <user>:<group> /host

create docker portainer:
cd /host/app && touch docker-compose.portainer.yaml
nano docker-compose.portainer.yaml -> Copy and paste
docker-compose -f docker-compose.portainer.yaml up
Goto http://<machine_ip>:9000 and set up password

Create a new stack in Portainer for backup
Set env variables:
    - ENCRYPTION_KEY -> set some value
Goto http://<machine_ip>:8200
Default password is 'changeme'
Change the password in Settings -> Change server passphrase

Add private key for nordVPN
touch /host/app/secrets/nordlynx_privatekey.txt
How to get private key: [Instructions](https://github.com/bubuntux/nordlynx#how-to-get-your-private_key)
Add your private key in the file.
nano /hosts/app/secrets/nordlynx_privatekey.txt
Create a new stack in Portainer for torrenting
Wait until both containers are up and running
Goto http://<machine_ip>:8080
First time password can be found in the qbittorrent container logs.
Change the password from the web UI -> Settings -> WebUI -> Authentication.
Change the network interface -> Settings -> Advanced -> Network interface. Should be wg0, but in any case goto [text](https://ipleak.net/) and click 'Torrent Address detection' and copy the magnet link. Paste the link in qbittorrent and verify that the IP is different from your own IP.

Create a new stack in Portainer for the library automation
Set up all applications from the stack

Create a new stack in Portainer for Plex
Set up a server

Create a new stack in Portainer for FileBrowser
Goto http://<machine_ip>:8000
Update the password from Settings

Schedule a daily backup for the config files -> /source/app/config
