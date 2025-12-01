# RoMM Easy Docker Setup

A simplified Docker Compose setup for [RoMM](https://github.com/rommapp/romm) - a ROM manager web application that allows you to scan, enrich, and browse your game collection.

## What is RoMM?

RoMM is a modern, web-based ROM manager that helps you:
- Organize your retro gaming collection
- Automatically fetch metadata and artwork from various providers
- Browse your games through a beautiful web interface
- Manage saves, states, and screenshots
- Support for multiple gaming platforms and emulators

## Features

- 🐳 **Easy Docker deployment** with pre-configured services
- 🗄️ **MariaDB database** for reliable data storage
- 🎮 **Multi-platform support** for 25+ gaming systems
- 🔌 **Multiple metadata providers** (ScreenScraper, IGDB, RetroAchievements, SteamGridDB)
- 📁 **Organized volume mappings** for easy ROM library management
- 🔒 **Secure configuration** with environment variables

## Supported Platforms

### Apple
- Mac, iOS, Bandai Pippin, Apple II

### Google
- Android

### Microsoft
- DOS, Windows, Xbox, Xbox 360

### Nintendo
- Game Boy, Game Boy Advance, Game Boy Color
- NES, N64, 3DS, New 3DS, DS
- GameCube, Pokémon Mini, SNES
- Switch, Virtual Boy, Wii, Wii U

### Sony
- PlayStation 1-5, PlayStation Portable, PlayStation Vita

### SEGA
- Dreamcast, Game Gear, Master System
- Mega Drive, Pico, Saturn, SG-1000

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)

## Quick Start

1. **Clone this repository**:
   ```bash
   git clone https://github.com/BillyOutlast/romm-easy.git
   cd romm-easy
   ```

2. **Create environment file**:
   ```bash
   cp env.example .env
   ```

3. **Configure your environment** (see [Configuration](#configuration) below)

4. **Start the services**:
   ```bash
   docker compose up -d
   ```

5. **Access RoMM**:
   - Open your browser and go to `http://localhost` (port 80)
   - The application will be available once the containers are running

## Configuration

### Required Configuration

Edit your `.env` file and configure the following **required** settings:

```bash
# Database Configuration
DB_PASSWD=your_secure_database_password
DB_ROOT_PASSWD=your_secure_root_password

# RoMM Authentication
ROMM_AUTH_SECRET_KEY=your_32_character_hex_key
```

**Generate a secret key**:
```bash
openssl rand -hex 32
```

### ROM Library Paths

Update the ROM library paths in your `.env` file to point to your actual game collections:

```bash
# Example: Point to your actual ROM directories
Nintendo_NES_PATH=/home/user/roms/nes
Nintendo_SNES_PATH=/home/user/roms/snes
Sony_PlayStation_PATH=/home/user/roms/psx
# ... etc
```

### Optional: Metadata Providers

Configure metadata providers for enhanced game information and artwork:

```bash
# ScreenScraper (Free, registration required)
SCREENSCRAPER_USER=your_username
SCREENSCRAPER_PASSWORD=your_password

# IGDB (Free, API key required)
IGDB_CLIENT_ID=your_client_id
IGDB_CLIENT_SECRET=your_client_secret

# RetroAchievements (Free, API key required)
RETROACHIEVEMENTS_API_KEY=your_api_key

# SteamGridDB (Free, API key required)
STEAMGRIDDB_API_KEY=your_api_key
```

#### Getting API Keys

- **ScreenScraper**: Register at [screenscraper.fr](https://screenscraper.fr)
- **IGDB**: Create an app at [Twitch Developer Console](https://dev.twitch.tv/console)
- **RetroAchievements**: Get your API key from [retroachievements.org](https://retroachievements.org)
- **SteamGridDB**: Get your API key from [steamgriddb.com](https://www.steamgriddb.com/profile/preferences/api)

For detailed setup instructions, see the [RoMM documentation](https://docs.romm.app/latest/Getting-Started/Metadata-Providers/).

## Directory Structure

```
romm-easy/
├── docker-compose.yaml       # Docker services configuration
├── env.example              # Environment variables template
├── .env                     # Your environment configuration (create this)
└── romm/
    ├── config/
    │   └── config.yml      # RoMM application configuration
    ├── romm_resources/     # Cached images and metadata (auto-created)
    ├── romm_redis_data/    # Background task cache (auto-created)
    ├── assets/             # User uploads (saves, states) (auto-created)
    └── mysql_data/         # Database files (auto-created)
```

## Port Configuration

By default, RoMM is accessible on port 80. If you need to change the port, uncomment and modify the ports section in `docker-compose.yaml`:

```yaml
ports:
  - "8080:8080"  # Access RoMM at http://localhost:8080
```

## Managing the Service

### Start the services
```bash
docker compose up -d
```

### Stop the services
```bash
docker compose down
```

### View logs
```bash
docker compose logs -f romm
```

### Update to latest version
```bash
docker compose pull
docker compose up -d
```

## Troubleshooting

### Service won't start
- Check that all required environment variables are set in `.env`
- Ensure Docker and Docker Compose are properly installed
- Verify that the specified ROM library paths exist and are accessible

### Database connection issues
- Confirm database passwords match between RoMM and MariaDB services
- Wait for the database to fully initialize (check with `docker compose logs romm-db`)

### ROMs not appearing
- Verify ROM library paths in `.env` point to correct directories
- Check file permissions on ROM directories
- Review the `config.yml` file for any exclusion rules

### Can't access web interface
- Ensure port 80 is not used by another service
- Check firewall settings
- Verify containers are running with `docker compose ps`

## Security Notes

- Change default passwords in the `.env` file
- Use a strong, randomly generated `ROMM_AUTH_SECRET_KEY`
- Consider running behind a reverse proxy with SSL/TLS for external access
- Regularly backup your database and configuration

## Contributing

This is a community-maintained setup. Feel free to:
- Report issues
- Suggest improvements
- Submit pull requests

## License

This project is provided as-is under the MIT License. RoMM itself is licensed under the AGPL-3.0 License.

## Related Links

- [RoMM Official Repository](https://github.com/rommapp/romm)
- [RoMM Documentation](https://docs.romm.app/)
- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Documentation](https://docs.docker.com/compose/)

## Support

For RoMM-specific issues, please refer to the [official RoMM documentation](https://docs.romm.app/) or [GitHub repository](https://github.com/rommapp/romm).

For issues with this Docker setup, please open an issue in this repository.