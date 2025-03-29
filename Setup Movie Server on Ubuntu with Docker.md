Setup Movie Server on Ubuntu with Docker

sudo apt update && sudo apt upgrade -y

Install Docker:
sudo apt install -y docker.io docker-compose
sudo usermod -aG docker $USER
newgrp docker

Create docker-compose for qbittorrent and nordlynx -> docker-compose.torrent.yaml
How to get private key: [Instructions](https://github.com/bubuntux/nordlynx#how-to-get-your-private_key)
docker run --rm --cap-add=NET_ADMIN -e TOKEN={{{TOKEN}}} ghcr.io/bubuntux/nordvpn:get_private_key
create ~/app/secrets/nordlynx_privatekey.txt

In the WebUI change the login credentials.
In the Advanced change the Network interface to the one that shows results in ipleak.
Verify torrent is using VPN in https://ipleak.net/

Create docker compose for portainer -> docker-compose.portainer.yaml
Set up password in the web ui -> ip:9000

Create docker compose for library automation -> docker-compose.automation.yaml
Set up Prowlar authentication in the web ui -> ip:9696
Add new indexer
(After Radarr installation)
Add App -> Radarr
    - Prowlarr Server: http://prowlarr:9696
    - Radarr Server: http://radarr:7878
    - API Key: Found in Radarr -> Settings -> General
Verify in Radarr -> Settings -> Indexers that the indexer is visible

Set up Radarr authentication in the web ui -> ip:7878
Set up download clients -> qBittorrent
Create /media/movies folder
Fix permissions:
    - sudo chown -R 1000:1000 ~/downloads ~/media
    - sudo chmod -R 775 ~/downloads ~/media
In web UI -> Settings -> Media management -> Add root folder and set to /movies. This should map files to ~/media/movies on host.

Create docker compose for Plex -> docker-compose.plex.yaml
