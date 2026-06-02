# Architecture

## Overview

Uptime Kuma is used as the central monitoring system for my self-hosted homelab infrastructure.

The setup is split into two independent monitoring instances:

- 🇩🇪 **Uptime Kuma DE** – monitors the Germany / Mettmann homelab site
- 🇵🇱 **Uptime Kuma PL** – monitors the Poland homelab site

Both instances run independently, so each location can monitor its own local infrastructure and services.

---

## High-Level Design

```text
                    ┌────────────────────────┐
                    │      Homelab DE         │
                    │   Germany / Mettmann    │
                    └───────────┬────────────┘
                                │
                                ▼
                    ┌────────────────────────┐
                    │    Uptime Kuma DE       │
                    │  Proxmox LXC Container  │
                    │  systemd + Node.js/npm  │
                    └────────────────────────┘


                    ┌────────────────────────┐
                    │      Homelab PL         │
                    │        Poland           │
                    └───────────┬────────────┘
                                │
                                ▼
                    ┌────────────────────────┐
                    │    Uptime Kuma PL       │
                    │  Proxmox LXC Container  │
                    │  systemd + Node.js/npm  │
                    └────────────────────────┘

````
Instance Architecture

Each Uptime Kuma instance runs inside a Proxmox container.

-Component	Description

-Platform	Proxmox VE

-Container type	LXC

-Runtime	Node.js / npm
-Service manager	systemd
-Application path	/opt/uptime-kuma
-Service name	uptime-kuma.service
-Installation type	Non-Docker
-Current version	2.4.0

Docker and PM2 are not used in this setup.

Locations
🇩🇪 Uptime Kuma DE

The DE instance monitors services and devices related to the Germany / Mettmann homelab.

Typical monitored systems:

Proxmox VE
Home Assistant DE
Plex Media Server
Nginx Proxy Manager
AdGuard
Network devices
Local hosts
Public status pages
🇵🇱 Uptime Kuma PL

The PL instance monitors services and devices related to the Poland homelab.

Typical monitored systems:

Proxmox VE
Home Assistant PL
Backup services
NAS / storage services
Network devices
Local hosts
Public status pages
