# Architecture

## Overview

Uptime Kuma is used as the central monitoring system for my self-hosted homelab infrastructure.

The setup is split into two independent monitoring instances:

- 🇩🇪 **Uptime Kuma DE** – monitors the Germany / Mettmann homelab site
- 🇵🇱 **Uptime Kuma PL** – monitors the Poland homelab site

Both instances run independently, so each location can monitor its own local infrastructure and services.

## Monitoring Model

The two instances are split by geography, not by feature set.

- A monitor is created in the location where the service is hosted or where the failure should be detected first.
- If the same service matters in both places, the monitor can be duplicated in DE and PL on purpose.
- These duplicates are not accidental copies; they provide two viewpoints on the same service, which helps confirm whether a problem is local, regional, or global.
- Public status pages then show the same logical service from each site without mixing the alerts.

This keeps the documentation aligned with the operational reality: one system, two views, each with its own context.

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

| Component         | Description           |
| ----------------- | --------------------- |
| Platform          | Proxmox VE            |
| Container type    | LXC                   |
| Runtime           | Node.js / npm         |
| Service manager   | systemd               |
| Application path  | `/opt/uptime-kuma`    |
| Service name      | `uptime-kuma.service` |
| Installation type | Non-Docker            |
| Current version   | `2.4.0`               |


Docker and PM2 are not used in this setup.

Locations

🇩🇪 Uptime Kuma DE

The DE instance monitors services and devices related to the Germany / Mettmann homelab.

Typical monitored systems:

* Proxmox VE
* Home Assistant DE
* Plex Media Server
* Nginx Proxy Manager
* AdGuard
* Network devices
* Local hosts
* Public status pages

The PL instance monitors services and devices related to the Poland homelab.

Typical monitored systems:

* Proxmox VE
* Home Assistant PL
* Backup services
* NAS / storage services
* Network devices
* Local hosts
* Public status pages



Service Layout

Uptime Kuma is installed in:
````
/opt/uptime-kuma
````

Important directories:

````
/opt/uptime-kuma/
├── data/              # Database and configuration
├── dist/              # Frontend files
├── server/            # Backend server
├── node_modules/      # Node.js dependencies
├── package.json       # Application metadata / version
└── .git/              # Git repository after conversion

````
The most important folder is:
````
/opt/uptime-kuma/data
````

This folder contains the Uptime Kuma database and configuration.
It must be backed up before every update.
