# Media Server Stack

Complete Docker Compose stack for a self-hosted media server with automated management, monitoring, and VPN protection.

## 🎯 Features

- **Media Management**: Plex, Radarr, Sonarr, Bazarr
- **Download Clients**: qBittorrent (VPN-protected), SABnzbd
- **Indexers**: Prowlarr with FlareSolverr
- **Request Management**: Overseerr, Requestrr
- **Monitoring**: Tautulli, Notifiarr
- **Reading**: Kavita
- **Management**: Portainer
- **Auto-healing**: Health checks with automatic container restart

## 🚀 Quick Start

### Prerequisites

- Docker & Docker Compose installed
- VPN credentials (NordVPN, Surfshark, etc.)
- Sufficient storage for media

### Installation

1. Clone this repository:
```bash
git clone https://github.com/sergiofdezcano/media-server-stack.git
cd media-server-stack
```

2. Copy the example environment file:
```bash
cp .env.example .env
```

3. Edit `.env` with your VPN credentials:
```bash
nano .env
```

4. Create required directories:
```bash
mkdir -p /docker/{portainer,plex,radarr,sonarr,prowlarr,bazarr,sabnzbd,overseerr,notifiarr,requestrr,kavita,tautulli,gluetun,qbittorrent}
mkdir -p /vault/data/{media,torrents,usenet}
```

5. Start the stack:
```bash
docker compose up -d
```

## 📋 Services

| Service | Port | Description |
|---------|------|-------------|
| Plex | 32400 | Media server |
| Radarr | 7878 | Movie management |
| Sonarr | 8989 | TV show management |
| Prowlarr | 9696 | Indexer manager |
| Bazarr | 6767 | Subtitle management |
| qBittorrent | 8085 | Torrent client (VPN) |
| SABnzbd | 8080 | Usenet client |
| Overseerr | 5055 | Request management |
| Portainer | 9443 | Docker management |
| Tautulli | 8181 | Plex monitoring |
| Kavita | 5000 | Ebook reader |
| Requestrr | 4545 | Discord bot |
| FlareSolverr | 8191 | Cloudflare bypass |

## 🔒 Security

- VPN protection for torrent traffic via Gluetun
- Health checks on critical services
- Auto-healing container restarts
- Docker log rotation configured

## ⚙️ Configuration

### VPN Setup

Edit `.env` file:
```env
VPN_SERVICE_PROVIDER=nordvpn
VPN_TYPE=openvpn
OPENVPN_USER=your_username
OPENVPN_PASSWORD=your_password
SERVER_COUNTRIES=Denmark
```

### Health Checks

Services with automatic health monitoring:
- Radarr, Sonarr, Prowlarr, Bazarr
- SABnzbd, Overseerr, Portainer

The `autoheal` container automatically restarts unhealthy containers.

## 🛠️ Maintenance

### Update containers:
```bash
docker compose pull
docker compose up -d
```

### View logs:
```bash
docker compose logs -f [service_name]
```

### Restart a service:
```bash
docker compose restart [service_name]
```

## 📁 Directory Structure

```
/docker/          # Container configurations
/vault/data/
  ├── media/      # Media library
  ├── torrents/   # Torrent downloads
  └── usenet/     # Usenet downloads
```


## 📝 License

MIT License - feel free to use this for your own setup.
