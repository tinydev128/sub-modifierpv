# ⚡ 3x-ui Custom Sub Modifier & Smart Router
[🇮🇷 **نسخه فارسی (Persian)**](README-fa.md)

A highly optimized, multi-threaded subscription management middleware designed specifically for **3x-ui** panels. It acts as an intelligent proxy, modifying subscription configurations on-the-fly to bypass DPI, inject fragments, and enforce cipher suites based on specific keywords, all while dynamically adapting to the user's client app.

## ✨ Key Features

* **🧠 True Smart Router:** Share just one link with your users! The built-in dispatcher detects the client (`v2rayN`, `V2box`, `happ`, `PattNG`, `Flclash`, `NPV`, etc.) and automatically serves the correct format (**JSON, Base64, or YAML**) with optimal routing rules.

* **⚡ Zero-Latency Architecture:** Say goodbye to nested routing deadlocks! Powered by a multi-threaded Gunicorn backend, a single unified master service handles all traffic natively, dropping RAM usage to under 80MB while delivering sub-50ms response times.

* **🛡️ Custom Security Paths:** Protect your server from active probing! Define custom paths for both your 3x-ui panel and the router app, breaking away from standard, easily scannable `/sub/` or `/json/` endpoints.

* **🧱 DPI Bypass Engine (Finalmask):** Automatically injects advanced fragment rules to evade Deep Packet Inspection. Includes Standard schemas for Xray, Hybrid schemas for strict Sing-box cores, and native `fragment` objects for Mihomo/Clash.

* **🌐 Minimalist Web Dashboard:** If a user opens the link in a browser, they are greeted with an ultra-fast, beautifully designed 1-Click App Installer. It features auto Dark/Light mode and dynamically fetches your live **3x-ui Announcements** in real-time.

* **🔄 Built-in Version Manager:** Easily update to the latest GitHub release or rollback to stable tags using the built-in CLI tool.

## 🛠️ How It Works (The Core Logic)

This script acts as a "Man-in-the-Middle" between your 3x-ui panel and your users, but it operates highly selectively.

During the installation, you are asked to provide **Target Keywords** (e.g., `CFCDN, CFX, Premium`). When a user requests their subscription via the Smart Router, the following process occurs instantly:

### 1. Keyword Interception

The router parses the raw configuration from the 3x-ui panel and checks the `remark` (or name) of every single proxy node.

* If a node's name **contains** one of your target keywords, it gets intercepted for modification.

* If a node's name **does not contain** any target keywords (e.g., a direct IP tunnel, a custom Reality node, or a local server), it passes through **100% untouched**. This ensures your special configurations are never corrupted.

### 2. The Bypass Injection (For Targeted Nodes)

Once a target node is identified, the router applies a strict set of DPI-bypassing rules based on the detected client:

* **Fragment / Finalmask:** Injects specific TCP fragmentation rules to trick Deep Packet Inspection (e.g., `100-200` lengths with `10-20` intervals).

* **Cipher Suites Override:** Strips default ALPNs and forces a highly secure, specific list of TLS Cipher Suites to mask the traffic fingerprint.

* **Client Fingerprint:** Forces the `fingerprint` to `unsafe` (for V2ray cores) or `chrome` (for Clash/Mihomo cores).

* **Mux Disabling:** Forcefully disables multiplexing (`mux: -1`), which is often flagged by strict firewalls.

* **Routing & DNS Optimization:** Injects a clean, minimal DNS (8.8.8.8) and local routing rules directly into the JSON configuration to prevent DNS leaks and optimize connection speed.

### 3. Dynamic Format Delivery

Finally, the router checks the user's app (`User-Agent`):

* Sends **YAML** with native `fragment` parameters for Clash/Mihomo/Flclash. *(Beta)*

* Sends **JSON** with Hybrid Finalmask for Sing-box based clients (Happ, V2box, NPV, Incy, v2rayN).

* Sends **Base64** URI strings with appended query parameters for legacy or strict clients like PattN/PattNG.

## 🚀 Installation

Since this repository is private, you will need a **GitHub Personal Access Token (PAT)** with `repo` permissions to download the script.

Run the following command on your Ubuntu server (replace `YOUR_GITHUB_TOKEN` with your actual token):

```bash
sudo bash <(curl -H "Authorization: token YOUR_GITHUB_TOKEN" -Ls (https://raw.githubusercontent.com/tinydev128/sub-modifier/main/install.sh))
```
During the initial installation, you will be prompted to configure your base URL, target keywords, and custom security paths.

## 💻 CLI Usage & Management

After installation, simply type the following command in your terminal at any time to access the interactive management menu:

```
sudo sub-modifier

```

**From the interactive menu, you can:**

* ⚙️ Modify your panel's base URL, target keywords, custom security paths, and internal ports instantly.

* 🔄 Switch between different Finalmask profiles (New/Old).

* 🚀 Change or update the script version directly from GitHub releases.

* 📊 Check the live status of the Python service and monitor RAM usage.

* 📌 View client recommendations and usage guides.

* 🗑️ Completely uninstall the tool and clean up your server.

## ⚠️ Prerequisites

1. **JSON Subscription** must be enabled in your 3x-ui panel settings.

2. You need a valid SSL certificate (Fullchain and Privkey) for the HTTPS-enabled Smart Router to work.
