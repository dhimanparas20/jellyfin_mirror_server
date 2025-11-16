# Mirror Media Server Stack

A powerful, self-hosted, all-in-one download, media, and automation suite.  
Easily deploy your own media server, download manager, and automation tools using Docker Compose.

---

## 🚀 Features

- **Aria2**: Fast, multi-protocol downloader with web UI.
- **FileBrowser**: Web-based file manager (bundled with Aria2 UI).
- **Jellyfin**: Free, open-source media server for streaming your movies, TV, and anime.
- **Jellyseerr**: Media request and discovery platform for Jellyfin.
- **Radarr**: Automated movie downloader and organizer.
- **Sonarr**: Automated TV series downloader and organizer.
- **Sonarr-Anime**: Dedicated Sonarr instance for anime.
- **qBittorrent**: Powerful torrent client with modern web UI (Vuetorrent mod included).
- **Jackett**: Torrent indexer aggregator for Radarr/Sonarr.
- **Flaresolverr**: Bypass Cloudflare for indexers that require it.

---

## 📁 Directory Structure

```
project-root/
├── cache/
├── config/
│   ├── aria2c/
│   │   └── rclone.conf
│   ├── jellyfin/
│   ├── jellyseerr/
│   ├── radarr/
│   ├── sonarr/
│   ├── sonarr-anime/
│   ├── qbittorrent/
│   └── jackett/
├── data/
│   └── media/
│       ├── movies/
│       ├── tv/
│       └── anime/
├── torrents/
├── compose.yml
└── README.md
```

---

## 🛠️ Quick Start

### 1. **Clone the Repository**

```sh
git clone <your-repo-url>
cd <repo-folder>
```

---

### 2. **Set Permissions (Important!)**

Ensure the correct permissions for all persistent folders:

```sh
sudo chown -R 1000:1000 ./downloads ./torrents ./config ./data
sudo chmod -R 775 ./downloads ./torrents
sudo chmod -R 755 ./config ./data
```

---

### 3. **Configure Environment Variables**

- Edit `compose.yml` to set your preferred usernames, passwords, secrets, and domains.
- For Aria2, set `ARIA2_USER`, `ARIA2_PWD`, and `RPC_SECRET`.
- For Caddy/Aria2, set the `DOMAIN` variable to your domain (ensure DNS is set up).

---

### 4. **Edit the Caddyfile**

- Edit the Caddyfile for your domain and service needs:
  ```sh
  nano ./config/aria2c/SecureCaddyfile
  ```
  - Change the **first line** to your domain (e.g., `t3.privatedns.org {`).
  - Make sure your domain's DNS A record points to your server's public IP.

---

### 5. **Start All Services**

```sh
docker compose up -d
```

---

### 6. **Replace the Caddyfile in the Container**

After the services have started, run the following commands to install `nano` (optional), backup the original Caddyfile, and copy your edited Caddyfile into the container:

```sh
sudo docker exec -it aria2c apt update -y
sudo docker exec -it aria2c apt install nano -y
sudo docker exec -it aria2c mv /usr/local/caddy/SecureCaddyfile /usr/local/caddy/SecureCaddyfile_old
sudo docker cp ./config/aria2c/SecureCaddyfile aria2c:/usr/local/caddy/SecureCaddyfile
```

---

### 7. **Restart Caddy Service**

```sh
sudo docker compose restart aria2c
```

---

## ✅ **That's it! Your mirror server is live.**

---

## 🌐 Service Endpoints & Usage

| Service         | URL / Port           | Default Credentials / Notes                        | Usage Summary |
|-----------------|---------------------|----------------------------------------------------|---------------|
| **Aria2 UI**    | http://<host>:80     | Set via `ARIA2_USER`/`ARIA2_PWD`                   | Download manager for HTTP, FTP, BitTorrent, Metalink. Web UI included. |
| **FileBrowser** | http://<host>:80/files | admin/admin for first time                       | Browse, upload, and manage files in your data directory. |
| **Jellyfin**    | http://<host>:8096   | Create user on first launch                        | Stream your movies, TV, and anime to any device. |
| **Jellyseerr**  | http://<host>:5055   | Create admin on first launch                       | Request movies/series, integrates with Radarr/Sonarr/Jellyfin. |
| **Radarr**      | http://<host>:7878   | Create admin on first launch                       | Automated movie downloads via torrents. |
| **Sonarr**      | http://<host>:8989   | Create admin on first launch                       | Automated TV series downloads via torrents. |
| **Sonarr-Anime**| http://<host>:8990   | Create admin on first launch                       | Dedicated for anime series automation. |
| **qBittorrent** | http://<host>:45789  | Username: `admin` / Password: `see container logs` | Modern torrent client with Vuetorrent UI. Change password after first login! |
| **Jackett**     | http://<host>:9117   | Set on first launch                                | Add torrent indexers for Radarr/Sonarr. |
| **Flaresolverr**| http://<host>:8191   | N/A                                                | Used by Jackett to bypass Cloudflare. No direct UI. |

---

## 🧩 Service Details & First-Time Setup

### **Aria2 + FileBrowser**
- **Purpose:** Download files via HTTP, FTP, BitTorrent, Metalink. Manage files via web UI.
- **Access:** http://<host>:80
- **Credentials:** Set via `ARIA2_USER` and `ARIA2_PWD` in `compose.yml`.
- **Tips:** Use FileBrowser for file management and uploads.

### **Jellyfin**
- **Purpose:** Media server for streaming movies, TV, anime.
- **Access:** http://<host>:8096
- **First Run:** Create an admin user and point your libraries to `/media` (mapped to `./data/media`).
- **Tips:** Use Jellyfin apps for TV, mobile, or web.

### **Jellyseerr**
- **Purpose:** Media request platform for users to request movies/series.
- **Access:** http://<host>:5055
- **First Run:** Create an admin account, connect to Jellyfin, Radarr, and Sonarr in settings.
- **Tips:** Invite users to request content, approve/deny requests as admin.

### **Radarr**
- **Purpose:** Automated movie downloads.
- **Access:** http://<host>:7878
- **First Run:** Set up root folder as `/movies` (mapped to `./data/media/movies`), add indexers (via Jackett), and connect to qBittorrent.
- **Tips:** Use Jackett for torrent indexers, set up quality profiles.

### **Sonarr**
- **Purpose:** Automated TV series downloads.
- **Access:** http://<host>:8989
- **First Run:** Set up root folder as `/tv` (mapped to `./data/media/tv`), add indexers (via Jackett), and connect to qBittorrent.

### **Sonarr-Anime**
- **Purpose:** Dedicated Sonarr instance for anime.
- **Access:** http://<host>:8990
- **First Run:** Set up root folder as `/anime` (mapped to `./data/media/anime`), add anime-specific indexers, and connect to qBittorrent.

### **qBittorrent**
- **Purpose:** Torrent client for automated and manual downloads.
- **Access:** http://<host>:45789
- **Credentials:** Username: `admin`, Password: `adminadmin` (change after first login!)
- **Tips:** Used by Radarr/Sonarr for automated downloads. Supports Vuetorrent UI mod.

### **Jackett**
- **Purpose:** Aggregates torrent indexers for Radarr/Sonarr.
- **Access:** http://<host>:9117
- **First Run:** Add your favorite indexers, copy each Torznab feed URL, and add them individually to Radarr/Sonarr.
- **Tips:** Do **not** use the `/all` endpoint; add each indexer separately.

### **Flaresolverr**
- **Purpose:** Bypass Cloudflare for indexers that require it.
- **Access:** No web UI. Used by Jackett automatically.
- **Tips:** No configuration needed unless troubleshooting indexer access.

---

## ⚡ Usage Workflow

1. **Download files manually:** Use Aria2 UI or FileBrowser.
2. **Automated movie/TV/anime downloads:**  
   - Users request content via Jellyseerr.
   - Admin approves requests.
   - Radarr/Sonarr/Anime fetch torrents via Jackett and send to qBittorrent.
   - qBittorrent downloads files to `./torrents`.
   - Radarr/Sonarr moves completed files to `./data/media/movies`, `./data/media/tv`, or `./data/media/anime`.
   - Jellyfin indexes new media for streaming.

3. **Stream your media:** Use Jellyfin web or apps.

---

## 🔒 Security & Best Practices

- **Change all default passwords** (especially qBittorrent and Aria2).
- **Use HTTPS** (via Caddy or reverse proxy) for public access.
- **Restrict ports** on your firewall if not using a reverse proxy.
- **Back up your `config/` and `data/` folders** regularly.

---

## 🛠️ Maintenance & Troubleshooting

- **Restart all services:**  
  `docker compose restart`
- **View logs for a service:**  
  `docker compose logs <service-name>`
- **Update images:**  
  `docker compose pull && docker compose up -d`
- **Fix permissions:**  
  ```
  sudo chown -R 1000:1000 ./downloads ./torrents ./config ./data
  sudo chmod -R 775 ./downloads ./torrents
  sudo chmod -R 755 ./config ./data
  ```

---

## 📝 Notes

- **Do not use the Jackett `/all` endpoint** in Radarr/Sonarr. Add each indexer individually.
- **Anime automation:** Use the dedicated Sonarr-Anime instance for best results.
- **Plex is commented out** because it does not support remote hosting in this setup.

---

## 🤝 Contributing

Pull requests and suggestions are welcome!  
Fork and adapt for your own needs.

---

## 📞 Support

For issues or questions, open an issue on GitHub or contact the maintainer.
