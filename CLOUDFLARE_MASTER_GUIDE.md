# Cloudflare Master Hosting Guide

This guide covers the two best ways to host applications using Cloudflare:
1.  **Cloudflare Pages:** For Static Websites (HTML, React, Next.js, Portfolios).
2.  **Cloudflare Tunnel:** For Self-Hosted Servers (Raspberry Pi, Local Backend, APIs, Home Servers).

---

## Part 1: Cloudflare Pages (Static Hosting)
**Use this for:** Your Frontend code, React apps, or simple HTML/CSS sites. It is free, fast, and does NOT require a computer to be left on.

### Prerequisites
*   Your code should be uploaded to **GitHub**.

### Step-by-Step Guide
1.  **Log in to Cloudflare Dashboard** > **Compute (Workers & Pages)**.
2.  Click **Create an application** > **Pages** > **Connect to Git**.
3.  **Select your Repository:**
    *   Link your GitHub account.
    *   Choose the project repo you want to host.
    *   Click **Begin setup**.
4.  **Build Settings (Crucial):**
    *   **Project Name:** Your site name (e.g., `my-cool-site`).
    *   **Framework Preset:**
        *   If simple HTML: Choose `None`.
        *   If React/Vite: Choose `Vite` or `Create React App`.
        *   If Next.js: Choose `Next.js`.
    *   **Build Command:** (Cloudflare usually fills this, e.g., `npm run build`).
    *   **Output Directory:** (e.g., `dist`, `build`, or `public`).
5.  Click **Save and Deploy**.
6.  **Done!** Your site is live at `https://project-name.pages.dev`.

### Adding Your Custom Domain
1.  Go to your new Project Page in Cloudflare.
2.  Click **Custom Domains** > **Set up a custom domain**.
3.  Enter your domain (e.g., `www.indianiiot.com`).
4.  Cloudflare automatically updates the DNS.

---

## Part 2: Cloudflare Tunnel (Self-Hosting)
**Use this for:** Everything else. Backends, Databases, Raspberry Pis, or any software running on your own hardware.

### Prerequisites
*   A server (Laptop/PC/Pi) running your app (e.g. on `localhost:8080`).
*   A Cloudflare Account with a Domain added.

### Step 1: Open Zero Trust
1.  Go to **Cloudflare Dashboard**.
2.  Click **Zero Trust** (Left Sidebar).
3.  Go to **Networks** -> **Tunnels**.

### Step 2: Create Tunnel
1.  Click **Create a Tunnel**.
2.  Select **Cloudflared**.
3.  Name it (e.g. `home-server`).

### Step 3: Install Connector on Your Server
*   **For Linux (Raspberry Pi):**
    *   Select **Debian**.
    *   Copy the installation command (starts with `sudo cloudflared...`).
    *   Run it in your Pi's terminal.
*   **For Windows:**
    *   Select **Windows**.
    *   Download the `.msi` application.
    *   Open **Command Prompt (Admin)** and run the command shown.

### Step 4: Route Traffic (The Link)
Once the connector reads "Connected":
1.  Click **Next** (or "Configure" -> "Public Hostname").
2.  Click **Add a public hostname**.
3.  **Subdomain:** (Optional, e.g., `api` or `server`).
4.  **Domain:** Select your domain (e.g., `indianiiot.com`).
5.  **Service:**
    *   **Type:** `HTTP`
    *   **URL:** `localhost:YOUR_PORT` (e.g., `localhost:8080` for ThingsBoard, `localhost:3000` for React local).
6.  Click **Save**.

### Removing/Cleaning Tunnels
If you make a mistake or want to reinstall:
1.  **Stop Service:** `sudo cloudflared service uninstall` (on Linux).
2.  **Delete in Dashboard:** Delete the tunnel from the Zero Trust list.
3.  **Start Over:** Create new tunnel -> Get new token -> Install.

---

## Summary Cheat Sheet
| Requirement | Use Method |
| :--- | :--- |
| HTML / CSS Website | **Cloudflare Pages** |
| React / Vue / Next.js | **Cloudflare Pages** |
| Python / Node Backend | **Cloudflare Tunnel** |
| Raspberry Pi Projects | **Cloudflare Tunnel** |
| Home Assistant / CCTV | **Cloudflare Tunnel** |
