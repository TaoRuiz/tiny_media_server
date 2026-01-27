# Tiny media server

### ✨ Introduction :

A lightweight, self-hosted media server stack combining Jellyfin for streaming, Deluge for torrent downloads, and
Jackett for torrent indexing.  
All services run in Docker containers with shared volumes enabling automated workflow:
<pre>
    search torrents via Jackett 📋
                  ↓  
        auto-import to Deluge  
                  ↓  
              download
                  ↓  
instant availability in Jellyfin library 🍿
</pre>

**Stack components:**

- **Jellyfin**: Media server for streaming your personal video library
- **Deluge**: BitTorrent client with web interface
- **Jackett**: Torrent indexer aggregator (connects to multiple torrent sites)

### 📋 Requirements

- Docker compose
- ...and that's pretty much it! 🎉

### 🚀 Installation

```bash
# Clone or download this repository
git clone https://github.com/TaoRuiz/tiny_media_server.git
cd awesome_media_server

# Start services
docker compose up -d
```

### 🐎 Quick start

1. **Configure Jellyfin** (http://localhost:8096)
    - Complete initial setup wizard
    - Add media library pointing to `/media`
    - Start streaming your content

2. **Configure Deluge** (http://localhost:8112)
    - Default password: `deluge`
    - Set downloads folder to `/downloads`
    - Enable AutoAdd plugin in Preferences → Plugins
    - Configure AutoAdd: Watch folder: `/torrent-files`

3. **Configure Jackett** (http://localhost:9117)
    - Add your preferred torrent indexers
    - Set blackhole directory to `/torrent-files`

4. **Enjoy 🍿**
    - Choose your favorite indexer
    - Browse media
    - Download torrent file to the blackhole
    - Stream your movie from Jellyfin 🌈

ℹ️ If you encounter any problems or would like a more guided installation, please refer to the dedicated section (
below).

### 💻 Web Interfaces

| Service  | URL                   | Default Credentials    |
|----------|-----------------------|------------------------|
| Jellyfin | http://localhost:8096 | Configure on first run |
| Deluge   | http://localhost:8112 | Password: `deluge`     |
| Jackett  | http://localhost:9117 | No authentication      |

### 🐌 First-Time Setup Guide

#### 1. Configure Jellyfin

1. Access Jellyfin at http://localhost:8096
2. Fill in the requested fields and click "Next" until you reach the media library configuration page
3. **Add Media Library:**
    - Content type: `Movies` (or `Shows`, `Music`, etc.)
    - Display name: Choose a name (required)
    - Click the `+` button next to "Folders"
    - Enter or select: `/media`
    - Click "OK" and continue through the wizard
4. Complete the setup and log in
5. ✅ Jellyfin is ready!

![Add Library](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552474/jellyfin_01_setup_media_libraries_uyoykk.png)
![Setup Library](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552474/jellyfin_01_setup_media_libraries_uyoykk.png)
![Media Folder](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552476/jellyfin_03_setup_media_libraries3_wdsflc.png)

---

#### 2. Configure Deluge

1. Access Deluge at http://localhost:8112
2. **Login:**
    - Default password: `deluge`
    - ⚠️ You should change this password for security reasons
3. **Connection Manager:**
    - Connect to the `localclient` server
4. **Configure Downloads:**
    - Go to Preferences → Downloads
    - Set "Download to": `/downloads`
5. **Enable AutoAdd Plugin:**
    - Go to Preferences → Plugins
    - Check `AutoAdd` to install it
    - Click on `AutoAdd` in the left menu
    - Set "Watch Folder": `/torrent-files`
6. ✅ Deluge is ready!

![Deluge Connection](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552477/deluge_01_connection_manager_tlfrns.png)
![Download Settings](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552479/deluge_02_setup_download_directory_ceglvb.png)
![AutoAdd Plugin](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552479/deluge_02_setup_download_directory_ceglvb.png)
![AutoAdd Setup](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552481/deluge_04_add_watch_folder_ywx09r.png)

---

#### 3. Configure Jackett

1. Access Jackett at http://localhost:9117
2. **Add Indexer:**
    - Click "Add indexer"
    - Example: Select `Archive.org` (legal and free)
    - Configure and save
3. **Configure Blackhole:**
    - In indexer settings, set "Blackhole Directory" to: `/torrent-files`
4. ✅ Jackett is ready!

![Jackett Add Indexers](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552483/jackett_01_add_indexer_bdtqtz.png)
![Jackett Add Indexers 2](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552484/jackett_02_add_indexer2_samcgy.png)
![Jackett Blackhole Config](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552473/jackett_05_setup_blackhole_n9zq15.png)
![Blackhole Manual Search](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552486/jackett_03_manual_search_vdykoa.png)
![Blackhole Manual Search2](https://res.cloudinary.com/dujfskvzp/image/upload/v1769552487/jackett_04_manual_search2_y1dckb.png)

---

#### 4. Test the Complete Workflow 🎬

1. **Search for content:**
    - Open Jackett (http://localhost:9117)
    - Click "Manual Search" on your indexer
    - Browse available media
2. **Download:**
    - Find your desired content
    - Click "Save to Blackhole Directory"
3. **Automatic magic happens:**
    - ✨ Torrent file appears in Deluge automatically
    - 📥 Download starts
    - 📁 Once complete, file moves to `/downloads`
    - 🎥 Media appears in your Jellyfin library
4. **Stream:**
    - Open Jellyfin (http://localhost:8096)
    - Your content is ready to watch!
5. 🍿 **Enjoy!**

---

### ⚠️ Troubleshooting

**Torrent doesn't appear in Deluge:**

- Check AutoAdd plugin is enabled
- Verify watch folder is `/torrent-files`

**Media doesn't appear in Jellyfin:**

- Go to Jellyfin → Dashboard → Libraries → Scan Library
- Check file permissions in `/downloads`

**Deluge connection error:**

- Restart the container: `docker compose restart deluge`
- Check logs: `docker compose logs deluge`

### 🏗️ Architecture

```
┌──────────┐        ┌──────────┐        ┌──────────┐
│ Jackett  │        │  Deluge  │        │ Jellyfin │
│  :9117   │        │  :8112   │        │  :8096   │
└────┬─────┘        └────┬─────┘        └────┬─────┘
     │                   │                   │
     │    ┌──────────────┴────────┐          │
     │    │  AutoAdd              │          │
     ▼    ▼                       ▼          ▼
┌────────────────┐          ┌────────────────┐
│ torrent-files/ │          │   downloads/   │
│                │          │                │
│  (.torrent)    │          │  (media files) │
└────────────────┘          └────────────────┘
  Shared Volume              Shared Volume


Data flow:
1. Jackett saves .torrent → /torrent-files
2. Deluge watches /torrent-files → downloads to /downloads
3. Jellyfin scans /downloads → streams media
```

### 📁 Directory Structure

```
awesome_media_server/
├── docker-compose.yaml
└── volumes/
    ├── jellyfin-config/    # Jellyfin configuration
    ├── jellyfin-cache/     # Jellyfin cache
    ├── deluge-config/      # Deluge configuration
    ├── jackett-config/     # Jackett configuration
    ├── downloads/          # Shared: Downloaded media (Deluge → Jellyfin)
    └── torrent-files/      # Shared: Torrent files (Jackett → Deluge)
```

### 📏 User Permissions

Services run as UID 1000 / GID 1000 by default. Adjust if needed:

```yaml
environment:
  - PUID=1000
  - PGID=1000
```

### 📖 Documentation

- [Jellyfin](https://jellyfin.org/docs/)
- [Deluge](https://hub.docker.com/r/linuxserver/deluge)
- [Jackett](https://hub.docker.com/r/linuxserver/jackett)

### ⚠️ Disclaimer

This project is for educational purposes. Ensure you comply with local laws regarding torrenting and copyright.

### 📝 License

MIT