# Mirror Server

A powerful, self-hosted, all-in-one download, media, and file management suite, designed for easy deployment on your own server or cloud VM (such as AWS EC2).  
This project combines [Aria2](https://aria2.github.io/), [FileBrowser](https://filebrowser.org/), [Rclone](https://rclone.org/), [Jellyfin](https://jellyfin.org/), and [Caddy](https://caddyserver.com/) for secure, flexible, and user-friendly access to your downloads and media.

## Uses the follwing Repos
- [Aria2 UI](https://github.com/wahyd4/aria2-ariang-docker)
- [Aria2 Docker Compose](https://github.com/wahyd4/aria2-ariang-x-docker-compose/tree/master)

---

## 🚀 About

**Mirror Server** is a Docker-based stack that lets you:
- Download files via Aria2 (with a modern web UI)
- Browse and manage files with FileBrowser
- Stream your media library with Jellyfin
- Mount and index cloud storage with Rclone
- Serve everything securely over HTTPS with Caddy
- Easily expose endpoints on your own custom domains

This setup is ideal for personal media servers, seedboxes, or as a private cloud solution.

---

## 🛠️ Quick Start

### 1. **Clone the Repository**

```sh
git clone https://github.com/dhimanparas20/mirror_server
cd mirror_server
```

---

### 2. **Configure Your Domains**

- **Edit `compose.yml`:**
  ```sh
  nano compose.yml
  ```
  - Find the `DOMAIN` environment variable and set it to your domain (e.g., `mirror.yourdomain.com`).
  - **Make sure your domain's DNS A record points to your server's public IP.**

- **Edit the Caddyfile:**
  ```sh
  nano configs/SecureCaddyfile
  ```
  - Change the **first line** to your Jellyfin domain (e.g., `t1.yourdomain.com {`).
  - Make sure this domain also points to your server's public IP.

---

### 3. **Start the Services**

```sh
sudo docker compose up --build -d
```

---

### 4. **Replace the Caddyfile in the Container**

After the services have started, run the following commands to install `nano` (optional), backup the original Caddyfile, and copy your edited Caddyfile into the container:

```sh
sudo docker exec -it aria2c apt update -y
sudo docker exec -it aria2c apt install nano -y
sudo docker exec -it aria2c mv /usr/local/caddy/SecureCaddyfile /usr/local/caddy/SecureCaddyfile_old
sudo docker cp ./configs/SecureCaddyfile aria2c:/usr/local/caddy/SecureCaddyfile
```

---

### 5. **Restart Caddy Service**

```sh
sudo docker compose restart aria2c
```

---

## ✅ **That's it! Your mirror server is live.**

---

## 🌐 Endpoints

| Service         | URL                                                      |
|-----------------|---------------------------------------------------------|
| Aria2 Web UI    | https://go.mstservices.tech                             |
| FileBrowser     | https://go.mstservices.tech/files                       |
| Readonly Files  | https://go.mstservices.tech/ro                          |
| Rclone          | https://go.mstservices.tech/rclone                      |
| Gdrive Index    | https://luffy.mst-uploads.workers.dev/0:/               |
| Jellyfin Server | https://t1.mstservices.tech/web/                        |

> **Note:** Replace the above domains with your own if you changed them in the setup.

---

## 📁 Directory Structure

```
mirror_server/
├── cache/
├── configs/
│   ├── SecureCaddyfile
│   └── rclone.conf
├── data/
├── compose.yml
└── README.md
```

- **data/**: Your downloads and media files
- **configs/**: Configuration files for Caddy and Rclone
- **cache/**: Cache directory for services
- **compose.yml**: Docker Compose stack definition

---

## 📝 Additional Info

- **Security:** All endpoints are served over HTTPS via Caddy with automatic Let's Encrypt certificates.
- **Persistence:** All important data and configs are stored in bind-mounted volumes for easy backup and migration.
- **Customization:** You can add/remove services or tweak configs as needed for your use case.
- **Cloud Ready:** Works great on AWS EC2, DigitalOcean, or any VPS with Docker.

---

## 🧑‍💻 Contributing

Pull requests and suggestions are welcome!  
Feel free to fork and adapt for your own needs.

---

## 📞 Support

For issues or questions, open an [issue on GitHub](https://github.com/dhimanparas20/mirror_server/issues) or contact the maintainer.

---

## ⚡ Credits

- [Aria2](https://aria2.github.io/)
- [FileBrowser](https://filebrowser.org/)
- [Rclone](https://rclone.org/)
- [Jellyfin](https://jellyfin.org/)
- [Caddy](https://caddyserver.com/)

---

Enjoy your all-in-one media and download server! 🎉

## for permissions
- sudo chown -R 1000:1000 ./downloads
- sudo chmod -R 775 ./downloads
- sudo chown -R 1000:1000 ./config
- sudo chmod -R 755 ./config
