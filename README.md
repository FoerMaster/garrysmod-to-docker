# Gmod2Docker

<img width="567" height="268" alt="Frame 49" src="https://github.com/user-attachments/assets/cdc82264-d5bf-4cc1-927b-c84eaee85e51" />


Boilerplate for quick deployment of a Garry's Mod game server in a Docker container.

# Русский туториал
Тут: https://blog.vyatk1n.ru/content/blog/2026/01/gmod2docker
Русскоязычное соообщество разработчиков: https://gmod-dev.ru/


# EN Tutorial

## 🚀 Quick Start

### 1. Clone the repository
```bash
git clone https://github.com/yourusername/gmod2docker.git
cd gmod2docker
```

### 2. Start the server
```bash
docker-compose up
```

### Done 😁

## ⚙️ Configuration

### Environment Variables

Edit `docker-compose.yml` to configure your server:

```yaml
image: ghcr.io/foermaster/gserver-docker-source:main
#image: ghcr.io/foermaster/gserver-docker-source:dev # for using dev branch garry's mod server change `main` -> `dev`
environment:
  GMOD_PORT: "27015"          # Server port
  GMOD_TICKRATE: "32"         # Tickrate (16, 22, 33, 66, 100)
  GMOD_MAXPLAYERS: "16"       # Maximum players
  GMOD_GAMEMODE: "sandbox"    # Game mode (sandbox, darkrp, ttt, etc.)
  GMOD_MAP: "gm_construct"    # Starting map
  GMOD_INSECURE: "true"       # Disable VAC (true/false)
  POLL_INTERVAL: "2"          # File sync interval (seconds)
```

### Additional Launch Parameters

You can add any Source Dedicated Server parameters via `command`:

```yaml
command:
  - +hostname
  - "My Awesome Server"
  - +sv_password
  - "mypassword"
  - +rcon_password
  - "adminpass"
  - +sv_lan
  - "0"
```

## 📁 Project Structure

```
my_awesome_server/
├── docker-compose.yml    # Docker Compose configuration
├── src/                  # Your server source code
├── data/                 # Data folder (garrysmod/data and sv.db)
└── .dockerignore         # Files to exclude from sync
```

## 🔄 File Synchronization

All files from the project directory are automatically synced to `/gmodserv/garrysmod/` inside the container:

- **Check interval:** 2 seconds (configurable via `POLL_INTERVAL`)
- **Hot reload:** Changes are applied without restarting the container
- **Persistent storage:** The `data/` folder and `sv.db` file are saved on the host

### Adding Content

The `src` folder is essentially the `garrysmod` folder.
You can add an `addons` folder with your addons or a `gamemodes` folder with game modes.

If you want to replace an existing file, simply add it - Docker will replace the file and save the original for restoration!

Changes will be automatically picked up within 2 seconds.

## 🛠️ Useful Commands

```bash
# Run in background
docker-compose up -d

# View logs
docker-compose logs -f

# Stop server
docker-compose down

# Restart
docker-compose restart

# Update image
docker-compose pull
docker-compose up -d
```

## 📋 Requirements

- Docker 20.10+
- Docker Compose 2.0+
- 4GB RAM (minimum)
- 20GB free disk space

## 🐛 Troubleshooting

### Can't connect to server

1. Check that the container is running:
   ```bash
   docker ps
   ```

2. Check logs for errors:
   ```bash
   docker-compose logs gmod | grep -i error
   ```

3. Make sure the port is not in use:
   ```bash
   sudo lsof -i :27015
   ```

### Files are not syncing

1. Check `POLL_INTERVAL` (should be >= 1)
2. View sync script logs:
   ```bash
   docker-compose logs gmod | grep Hotload
   ```

### Server crashes on startup

1. Increase Docker memory (minimum 4GB)
2. Check addon compatibility
3. Try running with default settings

## 📚 Additional Information

- **Container source code:** [github.com/FoerMaster/gserver-docker-source](https://github.com/FoerMaster/gserver-docker-source)
- **Source Dedicated Server Wiki:** [developer.valvesoftware.com/wiki/Source_Dedicated_Server](https://developer.valvesoftware.com/wiki/Source_Dedicated_Server)
- **Garry's Mod Wiki:** [wiki.facepunch.com/gmod](https://wiki.facepunch.com/gmod)
---

**Made with ❤️ for the Garry's Mod community**

Donate TON: `UQCwkr7iZPGGA9TwCPeIE47taWvHoqA42DCa9B9SmxA9zsWj`
