# Tiny media server

### ✨ Introduction :

A lightweight, self-hosted media server stack combining Jellyfin for streaming, Deluge for torrent downloads, and
Jackett for torrent indexing.  
All services run in Docker containers with shared volumes enabling automated workflow:  
Search torrents with Jackett ➡️ Deluge will manage your downloads ➡️ Open your Jellyfin library ➡️ Enjoy 🍿

### 🚀 Quick start

#### 📋 Requirements:

- Docker compose
- ...and that's pretty much it! 🎉

#### 📦 Installation:

```bash
# Clone or download this repository
git clone https://github.com/TaoRuiz/tiny_media_server.git
cd tiny_media_server

# Start services
docker compose up -d
```

#### 🛠️ Setup:

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

### 💻 Web Interfaces

| Service  | URL                   | Default Credentials    | Misc                                                   |
|----------|-----------------------|------------------------|--------------------------------------------------------|
| Jellyfin | http://localhost:8096 | Configure on first run | [doc 📄](https://jellyfin.org/docs/)                   |
| Deluge   | http://localhost:8112 | Password: `deluge`     | [doc 📄](https://hub.docker.com/r/linuxserver/deluge)  |
| Jackett  | http://localhost:9117 | No authentication      | [doc 📄](https://hub.docker.com/r/linuxserver/jackett) |

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
```

### ⚠️ Disclaimer

This project is for educational purposes. Ensure you comply with local laws regarding torrenting and copyright.

### 📝 License

MIT