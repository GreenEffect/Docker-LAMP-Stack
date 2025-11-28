# 🐳 Docker LAMP Stack v2.0

Modern, cross-platform LAMP development environment with Docker.

![PHP](https://img.shields.io/badge/PHP-7.4%20to%208.3-777BB4?logo=php)
![MySQL](https://img.shields.io/badge/MySQL-8.0-4479A1?logo=mysql)
![MariaDB](https://img.shields.io/badge/MariaDB-10.3--10.6-003545?logo=mariadb)
![Apache](https://img.shields.io/badge/Apache-2.4-D22128?logo=apache)
![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20macOS%20%7C%20Windows-lightgrey)

## ✨ Features

- 🚀 **Multiple PHP versions** (7.4, 8.0, 8.1, 8.2, 8.3)
- 💾 **Multiple databases** (MySQL 8, MariaDB 10.3-10.6)
- 🔒 **HTTPS/SSL** support with auto-generated certificates
- 🎯 **Dual access modes**: Direct ports or reverse proxy (Nginx)
- 🔧 **phpMyAdmin** included and pre-configured
- 🌍 **Cross-platform**: Linux, macOS, Windows (Git Bash/WSL2/CMD)
- 📦 **Isolated projects**: Run multiple LAMP stacks simultaneously
- ⚡ **Auto-configuration**: Permissions and secrets handled automatically

## 📋 Prerequisites

- [Docker Desktop](https://www.docker.com/products/docker-desktop) installed and running
- **Windows users**: [Git Bash](https://git-scm.com/downloads) recommended (or WSL2)

## 🚀 Quick Start (30 seconds)

```bash
# 1. Clone or extract the project
cd Docker-LAMP/

# 2. Initialize (auto-configures everything)
./init.sh

# 3. Start the stack
docker compose up -d

# 4. Access your site
# → http://localhost:8080
# → phpMyAdmin: http://localhost:8080/lamp-mysql
```

**Default credentials:**
- MySQL User: `docker` / Password: `docker`
- MySQL Root: `root` / Password: `tiger`

## 📖 Documentation

- **[Quick Start Guide](docs/QUICKSTART.md)** - Get started in 5 minutes
- **[Examples](docs/EXAMPLES.md)** - Real-world use cases and configurations
- **[Troubleshooting](docs/TROUBLESHOOTING.md)** - Common issues and solutions
- **[Windows Guide](docs/WINDOWS.md)** - Windows-specific instructions
- **[Changelog](CHANGELOG.md)** - Version history

## ⚙️ Configuration

All configuration is done via the `.env` file (created by `init.sh`):

```bash
# Project name (must be unique per project)
COMPOSE_PROJECT_NAME=lamp

# PHP version (php8, php81, php82, php83)
PHPVERSION=php83

# Database (mysql8, mariadb103-106)
DATABASE=mysql8

# Ports (change if running multiple projects)
HTTP_PORT=8080
HTTPS_PORT=8443
MYSQL_PORT=3306

# Access mode
USE_REVERSE_PROXY=false  # false=ports, true=domains
```

### Switching PHP Versions

```bash
# Edit .env
PHPVERSION=php81  # or php8, php81, php82, php83

# Rebuild and restart
docker compose down
docker compose up -d --build
```

### Switching Database

```bash
# Edit .env
DATABASE=mariadb105  # or mysql8, mariadb103-106

# Clean database and restart
./lamp.sh clean-db
docker compose up -d
```

## 🛠️ Management Commands

Using the `lamp.sh` helper script:

```bash
./lamp.sh start          # Start containers
./lamp.sh stop           # Stop containers
./lamp.sh restart        # Restart containers
./lamp.sh logs [-f]      # View logs (follow mode optional)
./lamp.sh status         # Show container status
./lamp.sh info           # Show connection info

./lamp.sh shell          # Access webserver shell
./lamp.sh mysql          # Access MySQL shell

./lamp.sh rebuild        # Rebuild images
./lamp.sh clean          # Clean everything (containers + data)
./lamp.sh clean-db       # Clean database only
./lamp.sh ssl            # Generate SSL certificates
```

Or use Docker Compose directly:

```bash
docker compose up -d              # Start
docker compose down               # Stop
docker compose logs -f            # Logs
docker compose ps                 # Status
docker compose exec webserver bash   # Shell
```

## 🌐 Dual Access Modes

### Mode 1: Direct Port Access (Default)

Access via `http://localhost:8080`

**Pros:** Simple, no configuration needed  
**Cons:** Different ports per project

### Mode 2: Domain-Based Access (Reverse Proxy)

Access via `https://myproject.local`

**Setup:**

```bash
# 1. Edit .env
USE_REVERSE_PROXY=true
PROJECT_DOMAIN=myproject.local

# 2. Add to /etc/hosts (or C:\Windows\System32\drivers\etc\hosts)
127.0.0.1    myproject.local

# 3. Generate SSL certificates
./generate-ssl-certs.sh

# 4. Start with proxy profile
docker compose --profile proxy up -d
```

**Pros:** Clean URLs, HTTPS by default, professional  
**Cons:** Requires hosts file modification

## 🔀 Running Multiple Projects

Each project needs unique identifiers:

```bash
# Project 1
COMPOSE_PROJECT_NAME=lamp-project1
HTTP_PORT=8080
MYSQL_PORT=3306

# Project 2
COMPOSE_PROJECT_NAME=lamp-project2
HTTP_PORT=8081
MYSQL_PORT=3307
```

Or use reverse proxy mode with different domains:

```bash
# Project 1: https://project1.local
# Project 2: https://project2.local
```

## 📂 Project Structure

```
Docker-LAMP/
├── bin/                      # Dockerfiles for PHP and database versions
├── config/
│   ├── vhosts/              # Apache virtual hosts
│   ├── nginx/               # Nginx reverse proxy config
│   ├── php/                 # php.ini
│   ├── ssl/                 # SSL certificates
│   ├── phpmyadmin/          # phpMyAdmin config
│   └── initdb/              # MySQL init scripts (.sql, .sh)
├── data/
│   └── mysql/               # MySQL data (persistent)
├── logs/
│   ├── apache2/             # Apache logs
│   ├── mysql/               # MySQL logs
│   └── nginx/               # Nginx logs
├── www/                      # Your web files (document root)
├── docs/                     # Documentation
├── docker-compose.yml        # Docker Compose configuration
├── sample.env               # Environment template
├── init.sh                  # Initialization script (Linux/Mac/Git Bash)
├── init.bat                 # Initialization script (Windows CMD)
├── lamp.sh                  # Management helper script
├── generate-ssl-certs.sh    # SSL certificate generator
└── check-system.sh          # System requirements checker
```

## 🔧 Development Tools

### Xdebug

Xdebug is included. VS Code configuration in `.vscode/launch.json`:

```json
{
  "name": "Listen for Xdebug",
  "type": "php",
  "request": "launch",
  "port": 9003,
  "pathMappings": {
    "/var/www/html": "${workspaceFolder}/www"
  }
}
```

### Database Access

**Via phpMyAdmin:**
- URL: `http://localhost:8080/lamp-mysql`
- User: `docker` / Pass: `docker`

**Via CLI:**
```bash
./lamp.sh mysql
# or
docker compose exec database mysql -u docker -p
```

**External connection:**
- Host: `localhost`
- Port: `3306` (or value in `.env`)
- User: `docker` / Pass: `docker`

## 🐛 Troubleshooting

### Common Issues

**"Port already in use"**
```bash
# Change ports in .env
HTTP_PORT=8081
MYSQL_PORT=3307
```

**"Permission denied" (Linux/Mac)**
```bash
# Re-run init to fix permissions
./init.sh
```

**MySQL won't start: "data directory has files"**
```bash
# Clean database and restart
./lamp.sh clean-db
docker compose up -d
```

**phpMyAdmin: "Permissions incorrect"**
```bash
# Fix permissions
chmod 644 config/phpmyadmin/config.inc.php
docker compose restart webserver
```

**Scripts don't work: "readonly variable"**
```bash
# Variables already fixed in latest version
# If using old .env, update USER_ID/GROUP_ID variables
sed -i 's/^UID=/USER_ID=/' .env
sed -i 's/^GID=/GROUP_ID=/' .env
```

**More help:** See [Troubleshooting Guide](docs/TROUBLESHOOTING.md)

## 🖥️ Windows Users

**Recommended:** Use Git Bash for best experience

**3 Methods:**

1. **Git Bash** (recommended) - Full bash script support
2. **WSL2** - Linux experience on Windows
3. **CMD** - Basic support with `init.bat` and Docker commands

See [Windows Guide](docs/WINDOWS.md) for detailed instructions.

## 🔐 Security Notes

⚠️ **This stack is for LOCAL DEVELOPMENT only!**

For production:
- Change all default passwords
- Use proper SSL certificates (Let's Encrypt)
- Disable phpMyAdmin or restrict access
- Review and harden all configurations
- Use environment-specific configs

## 🆚 What's New in v2.0

### Major Changes from v1.x

✅ Removed Traefik (simplified stack)  
✅ Removed Ansible (direct configuration)  
✅ Added Nginx reverse proxy (optional)  
✅ Added dual access modes (ports vs domains)  
✅ Full Windows support (Git Bash/WSL2/CMD)  
✅ Auto-generated phpMyAdmin secrets  
✅ Fixed permission handling cross-platform  
✅ Improved documentation and examples  

See [CHANGELOG.md](CHANGELOG.md) for full history.

## 📦 Docker Images Used

- **PHP**: Custom builds from `php:7.4-apache` to `php:8.3-apache`
- **MySQL**: `mysql:8.0`
- **MariaDB**: `mariadb:10.3` to `mariadb:10.6`
- **Nginx**: `nginx:alpine`
- **phpMyAdmin**: Built into PHP images

## 🤝 Contributing

Contributions are welcome! Please feel free to submit issues and pull requests.

## 📄 License

This project is open source. Feel free to use, modify, and distribute.

## 🙏 Credits

Built with ❤️ for the PHP development community.

---

**Need help?** Check the [documentation](docs/) or open an issue!
