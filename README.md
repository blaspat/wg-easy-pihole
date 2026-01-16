WireGuard Easy with PiHole
===========================

### Features :
1. WireGuard VPN + UI (Wireguard-Easy)
2. Integrated with PiHole as a DNS server

## Installation
1. Install Docker
````
curl -fsSL https://get.docker.com -o get-docker.sh  
sudo sh get-docker.sh
````
2. Download Docker compose file
````
sudo mkdir -p /opt/docker/wgeasy-pihole
sudo curl -o /opt/docker/wgeasy-pihole/docker-compose.yml https://raw.githubusercontent.com/blaspat/wg-easy-pihole/refs/heads/main/docker-compose.yml
````
3. Start `wgeasy-pihole`
````
cd /opt/docker/wgeasy-pihole
sudo docker compose up -d
````
4. Add ufw rules  
 - If using reverse proxy
````
sudo ufw allow 51820
sudo ufw allow 53
````
  Then set reverse proxy for port 51822 (PiHole UI) and 51821 (WireGuard UI)
 - If not using reverse proxy
````
sudo ufw allow 51820
sudo ufw allow 51821
sudo ufw allow 51822
sudo ufw allow 53
````

## Port
- **51820** : WireGuard VPN port
- **51821** : WireGuard UI port
- **51822** : PiHole UI port
- **53** : PiHole DNS port (default DNS port)

## Accessing UI
- WireGuard Easy : http://127.0.0.1:51821
- PiHole : http://127.0.0.1:51822

## License
[WireGuard](https://www.wireguard.com/)  
[WireGuard-Easy](https://github.com/wg-easy/wg-easy)  
[PiHole](https://pi-hole.net/)  
