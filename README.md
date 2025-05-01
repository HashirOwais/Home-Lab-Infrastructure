# 🏠 Home Lab Infrastructure

## 📌 Overview

This home lab was built with the goal of self-hosting essential services and eliminating recurring costs for cloud storage and web hosting. Instead of paying monthly fees (e.g., $10/month across various cloud storage providers), I decided to invest in local infrastructure that gives me more control, higher performance, and hands-on experience with real-world systems.

---

## 🎯 Objectives

- **💾 Centralized Storage**: Replace third-party cloud storage with a self-hosted NAS, enabling high-speed file transfers using 10 Gigabit networking.
- **🌐 Host My Own Web API**: I’m developing a daycare management API and wanted to host it from home instead of relying on paid cloud platforms.
- **🔐 Learn Networking & Security**: One of the main motivators for this lab is to gain hands-on experience with networking, firewalling, and securing a home network from malicious traffic — including ad blocking.

---

## 🔧 Future Plans

- **🛡️ OPNsense Router**  
  Build a dedicated firewall/router using a repurposed Dell OptiPlex running OPNsense (based on HardenedBSD). This system will handle:
  - VPN access (using WireGuard or similar) to securely access my network while away
  - Ad blocking at the network level (using tools like Unbound + blocklists or Pi-hole)

- **📡 VPN Access on the Go**  
  Set up a secure VPN tunnel to access my NAS and services remotely without exposing them directly to the internet.

- **📁 High-Speed NAS**  
  Deploy a dedicated NAS system with 10GbE networking for ultra-fast home file transfers and media storage.

- **🖥️ Service Host Node**  
  Set up another Dell OptiPlex to run services such as:
  - SQL Server (for internal and API use)
  - Ubuntu Server running NGINX (as a reverse proxy)
  - Docker containers for various workloads including my daycare API and future apps

---

## 🚀 Tech Stack Summary

| Component      | Tech Used                      |
|----------------|-------------------------------|
| Router/Firewall| OPNsense (on OpenBSD)         |
| VPN            | WireGuard (planned)           |
| NAS            | TrueNAS or custom Ubuntu + ZFS|
| Web/API Hosting| NGINX, Docker, .NET API       |
| DB Server      | SQL Server                    |
| Virtualization | Proxmox VE                    |

---

## 🧠 Why This Project?

This home lab isn't just about saving money — it's a long-term learning platform. It allows me to build, break, and secure a real network while exploring virtualization, containerization, self-hosting, and enterprise tools — all from home.

---

Let me know if you want a version with markdown badges or emojis removed for a more minimalist style!
