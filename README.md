# WireGuard Easy + PiHole

Docker compose setup for running WireGuard VPN and PiHole DNS ad-blocker together on the same host.

## Features

- WireGuard VPN with web UI (wg-easy)
- PiHole DNS-level ad blocking
- DNS queries routed through WireGuard VPN clients via PiHole
- Healthchecks for reliable restarts

## Requirements

- A Linux host (Ubuntu/Debian recommended)
- Docker and Docker Compose
- Open ports: 51820/UDP, 51821/TCP, 51822/TCP, 53/UDP

## Quick Setup

```bash
# 1. Install Docker
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# 2. Create directory
sudo mkdir -p /opt/docker/wgeasy-pihole
cd /opt/docker/wgeasy-pihole

# 3. Download docker-compose.yml
sudo curl -o docker-compose.yml https://raw.githubusercontent.com/blaspat/wg-easy-pihole/main/docker-compose.yml

# 4. Create .env file (set your own values!)
sudo nano .env
# Add: HOST_IP=192.168.1.100 (your server's LAN IP)
# Add: PIHOLE_PASSWORD=your_secure_password_here
# Add: TZ=Asia/Jakarta (or your timezone)

# 5. Start
sudo docker compose up -d
```

## Configuration

Edit the `.env` file before starting:

| Variable | Default | Description |
|----------|---------|-------------|
| `HOST_IP` | _(none)_ | Your server's LAN IP. WireGuard clients use this as their DNS server (points to PiHole). **Required.** |
| `PIHOLE_PASSWORD` | _(none)_ | Admin password for PiHole UI. **Required.** |
| `TZ` | `UTC` | Timezone (e.g. `Asia/Jakarta`, `America/New_York`) |

## Ports

| Port | Protocol | Service |
|------|----------|---------|
| 51820 | UDP | WireGuard VPN |
| 51821 | TCP | WireGuard UI |
| 51822 | TCP | PiHole UI |
| 53 | UDP | PiHole DNS |

## Accessing the UIs

- **WireGuard UI:** http://your-server-ip:51821
- **PiHole Admin:** http://your-server-ip:51822/admin

## Firewall (UFW)

```bash
sudo ufw allow 51820/udp   # WireGuard VPN
sudo ufw allow 51821/tcp   # WireGuard UI
sudo ufw allow 51822/tcp   # PiHole UI
sudo ufw allow 53/udp      # PiHole DNS
sudo ufw reload
```

## Client Setup

After starting the services:

1. Open WireGuard UI at port 51821
2. Create a new client — download the `.conf` file
3. Import the config into your WireGuard client app
4. All DNS queries from the VPN client will be routed through PiHole

## Managing

```bash
# View logs
sudo docker compose logs -f

# Restart
sudo docker compose restart

# Stop
sudo docker compose down

# Update images
sudo docker compose pull
sudo docker compose up -d
```

## How It Works

Both containers run in `host` network mode so WireGuard can manage the VPN interface directly and PiHole can serve DNS on port 53. When a WireGuard client connects, `INIT_DNS` is set to the host IP so all the client's DNS queries go through the local PiHole instance — blocking ads and trackers at the network level for all VPN traffic.

## Credits

- [WireGuard](https://www.wireguard.com/)
- [wg-easy](https://github.com/wg-easy/wg-easy)
- [PiHole](https://pi-hole.net/)
