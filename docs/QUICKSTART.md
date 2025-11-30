# Quick Start Guide - Docker LAMP Stack

## 🚀 Get Started in 3 Steps

### Linux / macOS / Windows (Git Bash)

### 1️⃣ Initialize
```bash
./init.sh
```

### 2️⃣ Configure (Optional)
Edit `.env` if you need custom settings:
```bash
nano .env
```

### 3️⃣ Start
```bash
docker compose up -d
```

🎉 **Done!** Access your site at http://localhost:8080

---

### Windows (Command Prompt) - Alternative

### 1️⃣ Initialize
```batch
init.bat
```

### 2️⃣ Configure (Optional)
```batch
notepad .env
```

### 3️⃣ Start
```batch
docker compose up -d
```

**Note for Windows users:** See [WINDOWS.md](WINDOWS.md) for detailed Windows guide.

---

## 📖 Common Commands

```bash
# Start the stack
./lamp.sh start

# Start with reverse proxy (domain access)
./lamp.sh start --proxy

# Stop the stack
./lamp.sh stop

# View logs
./lamp.sh logs

# Open shell in container
./lamp.sh shell

# Open MySQL shell
./lamp.sh mysql

# Show connection info
./lamp.sh info

# Get help
./lamp.sh
```

---

## 🌐 Access Methods

### Method 1: Direct Port Access (Default)
- Website: http://localhost:8080
- phpMyAdmin: http://localhost:8080/lamp-mysql

### Method 2: Domain Access (Reverse Proxy)
1. Edit `.env`:
   ```bash
   USE_REVERSE_PROXY=true
   PROJECT_DOMAIN=myproject.local
   ```

2. Add to `/etc/hosts`:
   ```bash
   echo "127.0.0.1 myproject.local" | sudo tee -a /etc/hosts
   ```

3. Generate SSL certificates:
   ```bash
   ./generate-ssl-certs.sh
   ```

4. Start with proxy:
   ```bash
   ./lamp.sh start --proxy
   ```

5. Access: https://myproject.local

---

## 🔧 Multiple Projects

### Setup Project 1
```bash
cd ~/projects/project1
git clone https://github.com/GreenEffect/Docker-LAMP-Stack.git .
./init.sh
# Edit .env: HTTP_PORT=8080, COMPOSE_PROJECT_NAME=project1
./lamp.sh start
```

### Setup Project 2
```bash
cd ~/projects/project2
git clone https://github.com/GreenEffect/Docker-LAMP-Stack.git .
./init.sh
# Edit .env: HTTP_PORT=8081, COMPOSE_PROJECT_NAME=project2
./lamp.sh start
```

Both projects now run simultaneously! 🎊

---

## 📝 Configuration Cheat Sheet

### Change PHP Version
```bash
# In .env
PHPVERSION=php83  # Options: php8, php81, php82, php83, php84
```

### Change Database
```bash
# In .env
DATABASE=mysql8   # Options: mysql8, mariadb103-106
```

### Change Ports
```bash
# In .env
HTTP_PORT=8080
HTTPS_PORT=8443
MYSQL_PORT=3306
```

### Change Credentials
```bash
# In .env
MYSQL_ROOT_PASSWORD=mypassword
MYSQL_DATABASE=mydatabase
MYSQL_USER=myuser
MYSQL_PASSWORD=mypassword
```

---

## 🐛 Troubleshooting

### Port Already in Use
```bash
# Change port in .env
HTTP_PORT=8081
```

### Permission Denied
```bash
# Run init.sh again to fix USER_ID/GROUP_ID
./init.sh
./lamp.sh rebuild
```

### Database Won't Start
```bash
# Check logs
./lamp.sh logs database

# Clean and restart
./lamp.sh clean
./lamp.sh start
```

### Can't Access Site
```bash
# Check if running
./lamp.sh status

# Check which port
./lamp.sh info
```

---

## 📚 Need More Help?

Read the full README.md for detailed documentation:
```bash
cat README.md
```

Or open an issue on GitHub! 🙋‍♂️
