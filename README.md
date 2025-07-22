# Personal_Cloud
I don't wanna waste my money to buy some subcription for my photos and videos and documents. Like I don't wanna use any googles or icloud things. I am serious about my privacy so I make my own cloud which I can access anywhere in the world and it is super fast and actual size. 

# ☁️ Self-Hosted Nextcloud with Docker and Cloudflare Tunnel

This project allows you to self-host your own private cloud (Nextcloud) using Docker and secure it with a Cloudflare Tunnel, so it's accessible from anywhere without opening any router ports.

---

## 📁 Project Structure

```
nextcloud-docker/
├── docker-compose.yml
├── db/                  # MariaDB database volume
├── nextcloud/           # Nextcloud app volume
└── README.md
```

---

## 🐳 Docker Setup

### 1. Prerequisites

- Docker
- Docker Compose
- Cloudflare Account
- A domain (e.g. `tyfsadik.org`) added to Cloudflare

### 2. Docker Compose Configuration

Here's the `docker-compose.yml` used:

```yaml
version: '3.7'

services:
  db:
    image: mariadb
    restart: always
    container_name: nextcloud-db
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    volumes:
      - ./db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=StrongRootPass
      - MYSQL_PASSWORD=nextcloudpass
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud

  app:
    image: nextcloud
    restart: always
    container_name: nextcloud
    ports:
      - 8080:80
    volumes:
      - ./nextcloud:/var/www/html
    environment:
      - MYSQL_PASSWORD=nextcloudpass
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_HOST=db
    depends_on:
      - db
```

---

## 🚀 Running the Project

```bash
docker-compose up -d
```

- Visit: [http://localhost:8080](http://localhost:8080)
- Complete the Nextcloud installation in the browser.

---

## 🔁 Auto Start on Boot

Docker services restart automatically due to `restart: always`. To ensure Docker itself starts on boot:

```bash
sudo systemctl enable docker
```

---

## 🗃️ Backup Strategy

Since all Nextcloud files and the database are stored in mounted volumes (`./nextcloud` and `./db`), you can back them up by copying the whole folder to an external hard drive:

```bash
rsync -a --delete ~/nextcloud-docker/ /mnt/backup-drive/nextcloud-backup/
```

To restore, copy it back and run:

```bash
docker-compose up -d
```

---

## 🌍 Exposing Nextcloud via Cloudflare Tunnel

Use Cloudflare Tunnel to expose your Nextcloud without opening ports on your router.

### 🔧 Requirements

- Cloudflared installed: [Install Instructions](https://developers.cloudflare.com/cloudflared/install/)
- Your domain set up in Cloudflare

---

### 🛠️ Setup Steps

#### 1. Authenticate

```bash
cloudflared tunnel login
```

#### 2. Create a Tunnel

```bash
cloudflared tunnel create nextcloud-tunnel
```

#### 3. Configure the Tunnel

Create a config file:

```bash
mkdir -p ~/.cloudflared
nano ~/.cloudflared/config.yml
```

Paste:

```yaml
tunnel: nextcloud-tunnel
credentials-file: /home/YOUR_USERNAME/.cloudflared/nextcloud-tunnel.json

ingress:
  - hostname: cloud.tyfsadik.org
    service: http://localhost:8080
  - service: http_status:404
```

> Replace `cloud.tyfsadik.org` with your subdomain.

#### 4. Create DNS Record

In Cloudflare DNS panel:
- Type: CNAME
- Name: `cloud`
- Target: `nextcloud-tunnel-id.cfargotunnel.com`

Or use:

```bash
cloudflared tunnel route dns nextcloud-tunnel cloud.tyfsadik.org
```

#### 5. Start the Tunnel

```bash
cloudflared tunnel run nextcloud-tunnel
```

Now visit: [https://cloud.tyfsadik.org](https://cloud.tyfsadik.org)

---

### 🛡️ Run Tunnel as a Service (Optional)

```bash
sudo cloudflared service install
```

Tunnel will auto-start on boot.

---

## ✅ Final Notes

- Secure your instance with HTTPS via Cloudflare
- Add storage mounts for large media if needed
- Regularly backup `nextcloud/` and `db/` directories

---

**Made with ❤️ by TYF Sadik**


