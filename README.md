# NetPulse — Multi-PC Network Activity, Firewall & Security Suite ⚡

[![Docker Hub](https://img.shields.io/badge/Docker%20Hub-ilegend44%2Fnetpulse%3Alatest-2496ED?logo=docker&logoColor=white)](https://hub.docker.com/r/ilegend44/netpulse)
[![Platform](https://img.shields.io/badge/Platform-Windows%20Desktop%20EXE%20%7C%20Linux%20Docker-00e5ff)](#)

> **All-in-one distributed network traffic monitor, stateful application firewall, 18-detector security analyzer, hardware resource profiler, LAN/Wi-Fi scanner, and point-to-point speed benchmark. Deploy across Mini PCs via the official Docker Hub Image Repository ([`ilegend44/netpulse:latest`](https://hub.docker.com/r/ilegend44/netpulse)) and Docker Compose, or launch the standalone Windows Desktop `.exe` with a transparent Always-on-Top floating PIP widget.**

---

## 🌟 Highlights (Unified Feature Suite)

- **📊 Traffic Monitor & Historical Timeline:**
  - Dual RX/TX live stream graph (`#00e5ff` Download / `#f472b6` Upload in Default Cyber Dark, plus **White & Orange (Solar)**, **High-Contrast Light (White)**, **Sapphire Blue**, and **Emerald Matrix** themes).
  - `Mbps` / `MB/s` and `Bytes` / `Bits` / `Packets` toggles, selectable Timeframe Scales (`Live 60s`, `5 Min`, `15 Min`, `1 Hour`, `24 Hours`), interactive Timeline Scrubbing, and 3-column NetGlass Active Apps / Remote Geo-IP Hosts / Deep Packet & App Inspector breakdown.
  - **Snapshot Export (`📷`)** to PNG with optional IP/hostname masking, **🕒 Idle / Away** period detection & away transfer summaries, and **Multi-PC Live Columns** right on the overview.
- **📈 Traffic Usage Table & CSV Export:**
  - Breakdown by Applications, Publishers, Countries, Protocols/Traffic Types, and Hosts across `Today`, `7 Days`, `30 Days`, and `All Time` with `All | External WAN | Local LAN` scope filters and 1-click **CSV Export**.
- **🌍 Interactive World Geo Map:**
  - Plots external endpoint destinations geographically with Country Flag badges (`🇺🇸`, `🇩🇪`, `🇬🇧`, `🇫🇷`, `🇯🇵`, `🇸🇬`, `🇨🇦`, `🇦🇺`, `🇳🇱`, `🇮🇳`, `🇧🇩`) while keeping `🏠 Local LAN` and `🔒 Mesh VPN` buckets cleanly separated, plus a Static Markers mode.
- **🛡️ Protect (Stateful Windows Firewall & Profiles):**
  - Independent named **Firewall Profiles** (`Home`, `Work`, `Public Wi-Fi`, `Metered`, plus custom profiles), per-direction rules (`⬇ Inbound` / `⬆ Outbound`), **🟢 Click to Block**, **🟡 Ask to Connect** pending approval queue, and **🔴 Block All** lockdown with Windows service exceptions & `Restore Previous Mode`.
  - Select-All **Staged Bulk Actions**, per-app Bandwidth Quotas, and **30-Day Automatic Rule Cleanup**.
- **💻 Hardware Resources:**
  - Live per-core CPU load & MHz, RAM used/committed, Disk read/write `MB/s` & IOPS, GPU telemetry, and top 25 resource-consuming processes with executable publisher metadata.
- **📱 Network Scanner (LAN Inventory & Wi-Fi Evil Twin Shield):**
  - Discovers local LAN & Wi-Fi hardware with MAC vendor lookup, remembers both online and offline devices persistently, supports custom device names (`✏️`) & notes (`📝`), and monitors Wi-Fi BSSID spoofing + active RDP sessions.
- **🔔 Log Analysis (18-Detector Security Suite) & VirusTotal SHA-256:**
  - 18 built-in security detectors (First Network Access, ARP Spoof / Evil Twin, RDP Activity, DNS/Host Changes, Bandwidth Overages, File Reputation, etc.) with per-detector toggle & **24-hour Snooze**.
  - Local SHA-256 binary hashing and VirusTotal v3 API / web reputation lookups from both the Connection Inspector modal and Protect table.
- **🖥️ Standalone Windows Desktop App (`NetPulse_Latest.exe`):**
  - Custom cyber-shield icon, **Floating PIP Widget (`🗖 PIP`)** with **Always-on-Top Pin (`📌`)** and **Opacity Slider (`30%–100%`)**, plus database directory relocation, JSON backup/restore, and per-app Incognito privacy exclusions under `⚙️ Settings & DB`.
- **📡 Multi-PC Distributed Architecture & Point-to-Point Speedtest:**
  - Central Hub + unlimited Agent nodes with live Ping, Download, and Upload throughput testing to any target IP.

---

## 💻 Running the Windows Desktop App (`NetPulse_Latest.exe`)

Download and run [`NetPulse_Latest.exe`](./NetPulse_Latest.exe) directly:
```bash
./NetPulse_Latest.exe
```
- **Double-click `NetPulse_Latest.exe`** (or right-click → **Run as administrator** to enforce live `netsh advfirewall` rules).
- No Python or external dependencies required.
- **Switch to Floating Widget:** Click the **`🗖 PIP`** button in the top bar to switch to the Always-On-Top desktop meter (adjust opacity with the slider or click `📌` / `⛶` to expand back).
- **Connect to Remote Mini PC:** Click the server button in the top bar (`Local Hub ▼`) and enter your Mini PC IP (e.g. `http://192.168.1.100:8990`).

---

## 🔑 Generating `NETPULSE_AUTH_TOKEN` & Security Setup

The `NETPULSE_AUTH_TOKEN` is a pre-shared security token stored exclusively in your local `.env` file (which is gitignored) to authenticate communication between your Central Hub and all secondary Mini PC nodes.

### How to Generate a Secure Random Key:

- **Python (Cross-platform - Windows, Linux, macOS):**
  ```bash
  python -c "import secrets; print(secrets.token_hex(24))"
  ```
- **Linux / macOS / WSL (OpenSSL):**
  ```bash
  openssl rand -hex 24
  ```
- **Windows PowerShell:**
  ```powershell
  [System.Guid]::NewGuid().ToString("N")
  ```

### Setting up `.env`:
1. Copy the template file on each machine:
   ```bash
   cp .env.example .env
   ```
2. Paste your generated token into `NETPULSE_AUTH_TOKEN=` inside `.env`.
3. Use the **same** token on your Central Hub and all Mini PC Agent nodes.

---

## 🐳 Option 1: Deploying via Docker Compose (`docker-compose.yml`)

[`docker-compose.yml`](./docker-compose.yml) pulls the pre-built image directly from Docker Hub (`ilegend44/netpulse:latest`) so no source code is needed on your Mini PCs.

### A. On your Primary Machine (Runs as Central Hub)
1. Copy the template and configure `.env`:
   ```bash
   cp .env.example .env
   ```
   ```env
   NETPULSE_IMAGE=ilegend44/netpulse:latest
   NETPULSE_AUTH_TOKEN=your_shared_secret_key_here
   NODE_MODE=hub
   NODE_NAME=Central-Hub-MiniPC
   PORT=8990
   HEARTBEAT_INTERVAL=2
   ```
2. Pull the image and start the container:
   ```bash
   docker compose pull
   docker compose up -d
   ```
3. Open the web dashboard at `http://<CENTRAL_HUB_IP>:8990`.

### B. On Secondary Mini PCs (Run as Worker Agents)
1. Copy the template and configure `.env` to point to your Central Hub's IP:
   ```bash
   cp .env.example .env
   ```
   ```env
   NETPULSE_IMAGE=ilegend44/netpulse:latest
   NETPULSE_AUTH_TOKEN=your_shared_secret_key_here
   NODE_MODE=agent
   NODE_NAME=LivingRoom-MiniPC
   HUB_URL=http://<CENTRAL_HUB_IP>:8990
   PORT=8990
   HEARTBEAT_INTERVAL=2
   ```
2. Pull the image and start the container:
   ```bash
   docker compose pull
   docker compose up -d
   ```

### Useful Docker Compose Commands

| Action | Command |
|---|---|
| **Pull Latest Image from Docker Hub** | `docker compose pull` |
| **Start in Background** | `docker compose up -d` |
| **View Live Logs** | `docker compose logs -f` |
| **Check Container Status** | `docker compose ps` |
| **Restart Container** | `docker compose restart` |
| **Stop & Remove Container** | `docker compose down` |

---

## 📦 Option 2: Deploying Directly from Docker Hub ([`ilegend44/netpulse:latest`](https://hub.docker.com/r/ilegend44/netpulse))

The official pre-built Linux (`linux/amd64`) container image is hosted on Docker Hub at **[https://hub.docker.com/r/ilegend44/netpulse](https://hub.docker.com/r/ilegend44/netpulse)**.

### Pulling & Running the Image on Any Mini PC (`docker pull` + `docker run`)

Deploy on any PC with a single command:

```bash
# Pull the latest image from Docker Hub:
docker pull ilegend44/netpulse:latest
```

- **Run as Central Hub (Linux — Host Networking for full NIC inspection):**
  ```bash
  docker run -d \
    --name netpulse \
    --restart unless-stopped \
    --network host \
    --cap-add NET_ADMIN \
    --cap-add NET_RAW \
    --env-file .env \
    -e NODE_MODE=hub \
    -e NODE_NAME=Central-Hub \
    ilegend44/netpulse:latest
  ```

- **Run on Windows / macOS Docker Desktop (Port Mapping mode):**
  ```bash
  docker run -d \
    --name netpulse \
    --restart unless-stopped \
    -p 8990:8990 \
    --env-file .env \
    -e NODE_MODE=hub \
    -e NODE_NAME=Desktop-Hub \
    ilegend44/netpulse:latest
  ```

- **Run on a Secondary Mini PC as an Agent:**
  ```bash
  docker run -d \
    --name netpulse-agent \
    --restart unless-stopped \
    --network host \
    -e NODE_MODE=agent \
    -e NODE_NAME=MiniPC-02 \
    -e HUB_URL=http://<CENTRAL_HUB_IP>:8990 \
    -e NETPULSE_AUTH_TOKEN=your_shared_secret_key_here \
    ilegend44/netpulse:latest
  ```

---

## 📊 API Endpoints Reference

- `GET /api/nodes` & `GET /api/self` — Lists all registered PCs, IPs, live bandwidth, and idle state.
- `GET /api/usage/summary` & `GET /api/usage/export-csv` — App/publisher/country/protocol breakdown & CSV export.
- `GET /api/geomap` — Country-level destination geography & separated LAN/Unknown buckets.
- `GET /api/hardware` — CPU per-core/MHz, RAM, Disk I/O, GPU telemetry, and top 25 active processes.
- `GET/POST /api/firewall/rules`, `/profile`, `/restore-mode`, `/block`, `/unblock`, `/quota`, `/master-toggle`, `/allow-all`, `/resolve-prompt`, `/bulk-apply`, `/cleanup`.
- `GET /api/network/things` & `POST /api/network/things/update` — Persistent LAN inventory & Wi-Fi BSSID status.
- `GET /api/alerts`, `POST /api/alerts/snooze`, `/mark-read`, `/detector-toggle` — Security log & 18 detectors.
- `POST /api/reputation/scan` & `POST /api/settings/virustotal` — Local SHA-256 + VirusTotal reputation check.
- `GET/POST /api/incognito`, `GET /api/system/health`, `GET /api/storage/backup`, `POST /api/storage/restore`, `/relocate`.
- `POST /api/speedtest/run?target_ip=<TARGET_PC_IP>&size_mb=10` — Point-to-point speedtest from Hub to target IP.
