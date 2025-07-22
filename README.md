# Personal_Cloud
I don't wanna waste my money to buy some subcription for my photos and videos and documents. Like I don't wanna use any googles or icloud things. I am serious about my privacy so I make my own cloud which I can access anywhere in the world and it is super fast and actual size. 

# 🚀 Nextcloud Docker Setup

A production-ready, portable, and easy-to-deploy **Nextcloud** environment using Docker and Docker Compose.

---

## 📦 Project Overview

This setup includes:

- 🗂️ **Nextcloud** (v31.0.7) – self-hosted file sync and sharing platform  
- 🐬 **MariaDB** (v10.5) – reliable database backend  
- 🛠️ **Persistent storage** for data and configs  
- 🔄 **Automatic container restarts**  
- 🌐 Access via `http://localhost:8080`  

---

## 🧰 Prerequisites

Make sure you have the following installed:

- [Docker Engine](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- At least **2 GB of free disk space**

---

## 📁 Folder Structure

```
nextcloud-docker/
├── docker-compose.yml       # Compose file with app + db services
├── .gitignore               # Exclude unnecessary files from Git
└── README.md                # This documentation
```

---

## ⚙️ Setup Instructions

### 1. Clone the repository

```bash
git clone https://github.com/tyfsadik/nextcloud-docker.git
cd nextcloud-docker
```

### 2. Start the environment

```bash
docker-compose up -d
```

This will:

- Pull the required images
- Start the MariaDB and Nextcloud containers
- Persist data in `./db` and `./nextcloud` on your host

### 3. Access Nextcloud

Open your browser and go to:

```
http://localhost:8080
```

Or replace `localhost` with your server’s IP or domain.

### 4. Complete Web Setup

Use the following details when prompted for database configuration:

- **Database user**: `nextcloud`
- **Database name**: `nextcloud`
- **Database password**: `nextcloudpass`
- **Database host**: `db`

Then finish creating your admin account.

---

## 💾 Data Persistence & Backup

- 🐬 **Database** stored in: `./db`
- 📁 **Nextcloud data/configs** stored in: `./nextcloud`

✅ Back up both folders regularly to avoid data loss.

---

## 📦 Stopping & Restarting

To **stop** the containers:

```bash
docker-compose down
```

To **start** again:

```bash
docker-compose up -d
```

---

## 🛠️ Troubleshooting

Check logs:

```bash
docker-compose logs -f
```

Enter Nextcloud container:

```bash
docker exec -it nextcloud bash
```

Check port conflicts (port 8080 should be free).  

---

## 🔐 Security Tips

- Change the default passwords in `docker-compose.yml` before production use.
- Use HTTPS via a reverse proxy like **Nginx** or **Caddy**.
- Block unused ports with a firewall.

---

## 🌐 Want HTTPS?

Use a reverse proxy:

- [Nginx Proxy Manager](https://nginxproxymanager.com/)
- [Caddy](https://caddyserver.com/)
- [Traefik](https://doc.traefik.io/)

---

## 👤 Created by

**MD. Taki Yasir Faraji Sadik** – [tyfsadik.org](https://tyfsadik.org)

---

## 📝 License

MIT License

---

## 📚 References

- [Nextcloud Docker Image](https://hub.docker.com/_/nextcloud)
- [MariaDB Docker Image](https://hub.docker.com/_/mariadb)
- [Docker Compose Docs](https://docs.docker.com/compose/)

