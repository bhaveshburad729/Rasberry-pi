# Project Hosting Documentation

**Domain:** `https://indianiiot.com`
**Host:** Raspberry Pi (Local)
**Method:** Cloudflare Tunnel (Zero Trust)
**Date Configured:** 2026-01-21

---

## 1. Architecture
Your website is hosted on your local Raspberry Pi but accessible to the world via a secure tunnel.
*   **No Port Forwarding:** Router ports are closed (Safe).
*   **No Dynamic IP Issues:** The tunnel connects outbound, so IP changes don't break it.
*   **Encryption:** HTTPS is handled automatically by Cloudflare.

**Traffic Flow:**
`User` -> `indianiiot.com (Cloudflare)` -> `Secure Tunnel` -> `Raspberry Pi (localhost:8080)`

---

## 2. Maintenance & Commands

The tunnel software (`cloudflared`) runs automatically in the background. It starts when the Pi turns on.

### Check Status
If the website is down, run these on the Pi terminal:
```bash
# 1. Is the Tunnel running?
sudo systemctl status cloudflared

# 2. Is ThingsBoard running?
sudo systemctl status thingsboard
```

### Restart Services
```bash
# Restart Tunnel
sudo systemctl restart cloudflared

# Restart ThingsBoard
sudo service thingsboard restart
```

---

## 3. Re-Installation Notes
If you ever wipe your SD card and need to reinstall, follow these steps (We had to use a manual method because of a repo error):

1.  **Download the Installer manually:**
    ```bash
    wget https://github.com/cloudflare/cloudflared/releases/latest/download/cloudflared-linux-arm64.deb
    ```
2.  **Install it:**
    ```bash
    sudo dpkg -i cloudflared-linux-arm64.deb
    ```
3.  **Link to Cloudflare:**
    *   Go to [Cloudflare Zero Trust Dashboard](https://one.dash.cloudflare.com).
    *   Create a new Tunnel.
    *   Copy the `sudo cloudflared service install eyJ...` command.
    *   Run it on the Pi.

---

## 4. Cloudflare Dashboard Management
To change settings (like adding a subdomain `admin.indianiiot.com`):
1.  Login to [Cloudflare Dashboard](https://dash.cloudflare.com).
2.  Go to **Zero Trust** (Left Sidebar) -> **Networks** -> **Tunnels**.
3.  Click `pi-tunnel` -> **Configure**.
4.  Go to **Public Hostname** to add/remove website routes.
