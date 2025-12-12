# Docker Media Server Configuration

This repository contains the complete Docker Compose configuration for a media server stack.

## Services Included

### Media Management
- **Plex** - Media server (port 32400)
- **Sonarr** - TV series management (port 8989)
- **Radarr** - Movie management (port 7878)
- **Bazarr** - Subtitle management (port 6767)
- **Overseerr** - Media request management (port 5055)
- **Tautulli** - Plex monitoring and statistics (port 8181)
- **Kavita** - Book/comic reader (port 5000)

### Download Clients
- **qBittorrent** - Torrent client (port 8085)
- **SABnzbd** - Usenet client (port 8080)
- **Gluetun** - VPN client for qBittorrent

### Indexers & Tools
- **Prowlarr** - Indexer manager (port 9696)
- **FlareSolverr** - Cloudflare bypass proxy (port 8191)

### Notifications & Requests
- **Requestrr** - Discord bot for media requests (port 4545)
- **Notifiarr** - Notification service (port 5454)

### Management
- **Portainer** - Docker management UI (port 9443)
- **Watchtower** - Automatic container updates

## Prerequisites

- Docker Engine
- Docker Compose v3.8+
- Required host directories:
  - `/docker/*` - Configuration files
  - `/vault/data/*` - Media and download storage

## Installation

1. Clone this repository:
   ```bash
   git clone <repository-url>
   cd docker-backup
   ```

2. Copy the example environment file and configure it:
   ```bash
   cp .env.example .env
   ```

3. Edit `.env` file with your actual values:
   - VPN credentials for Gluetun
   - Notifiarr API key

4. Create required directories on the host:
   ```bash
   mkdir -p /docker/{qbittorrent,gluetun,sonarr,radarr,prowlarr,bazarr,overseerr,tautulli,kavita,sabnzbd,plex,requestrr,notifiarr,portainer}
   mkdir -p /vault/data/{torrents,usenet,media}
   mkdir -p /docker/plex/{config,transcode}
   ```

5. Start the stack:
   ```bash
   docker-compose up -d
   ```

## Configuration Notes

### User Permissions
Most services run as PUID=1000 and PGID=1000. Adjust these if needed in the docker-compose.yml file.

### Time Zone
Default timezone is `Europe/Copenhagen`. Update `TZ` environment variables as needed.

### VPN Configuration
qBittorrent routes through Gluetun VPN container. Configure your VPN credentials in the `.env` file.

### Plex Hardware Acceleration
Plex has access to `/dev/dri` for hardware transcoding. Ensure your GPU drivers are properly installed.

### Networks
- `arrapps_default` - Main application network
- `qbittorrent_default` - VPN-isolated network for qBittorrent

## Ports Reference

| Service | Port | Description |
|---------|------|-------------|
| Plex | 32400 | Media server |
| Sonarr | 8989 | TV management |
| Radarr | 7878 | Movie management |
| Prowlarr | 9696 | Indexer manager |
| Bazarr | 6767 | Subtitle management |
| Overseerr | 5055 | Request management |
| Tautulli | 8181 | Plex statistics |
| Kavita | 5000 | Book reader |
| qBittorrent | 8085 | Torrent client |
| SABnzbd | 8080 | Usenet client |
| FlareSolverr | 8191 | Cloudflare proxy |
| Requestrr | 4545 | Discord bot |
| Notifiarr | 5454 | Notifications |
| Portainer | 9443 | Docker UI |

## Backup

To backup your configuration:
```bash
tar -czf docker-backup-$(date +%Y%m%d).tar.gz /docker/*/config
```

## Updates

Watchtower automatically checks for updates daily at 2 AM. To manually update:
```bash
docker-compose pull
docker-compose up -d
```

## Troubleshooting

### Check logs
```bash
docker-compose logs -f <service-name>
```

### Restart a service
```bash
docker-compose restart <service-name>
```

### Rebuild containers
```bash
docker-compose down
docker-compose up -d
```

## Security Notes

- Never commit the `.env` file with actual credentials
- Restrict network access to sensitive ports
- Keep Portainer secured with strong credentials
- Regularly update containers for security patches

## License

This configuration is provided as-is for personal use.
