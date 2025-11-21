# Homelab Services

This document provides an overview of the services running in the homelab.

## Service Inventory

The following table details the virtual machines, containers, and physical devices that make up the homelab environment.

| Hostname | IP Address | Description | Type | OS Family |
|---|---|---|---|---|
| `proxymanager` | 192.168.1.105 | Nginx Proxy Manager for reverse proxying | LXC | Debian |
| `uat-env` | 192.168.1.106 | User Acceptance Testing environment | LXC | Debian |
| `portainer` | 192.168.1.114 | Container management UI for Docker | VM | Debian |
| `tailscale` | 192.168.1.117 | Tailscale edge node for VPN access | VM | Debian |
| `cloudflare-connector` | 192.168.1.122 | Cloudflare Tunnel connector | VM | Debian |
| `gitlab-runner` | 192.168.1.123 | CI/CD runner for GitLab pipelines | LXC | Debian |
| `postgresql` | 192.168.1.124 | PostgreSQL database server | LXC | Debian |
| `wazuh` | 192.168.1.126 | Wazuh security monitoring platform (SIEM/XDR) | VM | Debian |
| `monitoring` | 192.168.1.134 | OTEL/LGTM/ELK monitoring stack | VM | Debian |
| `ansible` | 192.168.1.135 | Ansible automation controller | LXC | Debian |
| `mockapi` | 192.168.1.138 | Mock API server for development | LXC | Debian |
| `pi` | 192.168.6.4 | Raspberry Pi 4 | Physical | Debian |
| `howais` | 192.168.69.2 | prd-app-srv1 (Production Application Server 1) | VM | Red Hat |
