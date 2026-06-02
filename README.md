# Homelab Uptime Kuma

Self-hosted Uptime Kuma monitoring setup for my Proxmox-based homelab infrastructure in Germany and Poland.

## Overview

This repository documents my Uptime Kuma monitoring setup for two homelab locations:

- 🇩🇪 Uptime Kuma DE
- 🇵🇱 Uptime Kuma PL

Both instances run as non-Docker installations inside Proxmox containers.

## Tech Stack

- Linux
- Proxmox VE
- LXC
- Node.js
- npm
- systemd
- Uptime Kuma

## Instances

| Instance | Location | Runtime | Service | Path |
|---|---|---|---|---|
| Uptime Kuma DE | Germany | Node.js / npm | systemd | `/opt/uptime-kuma` |
| Uptime Kuma PL | Poland | Node.js / npm | systemd | `/opt/uptime-kuma` |

## Documentation

- [Architecture](architecture.md)
- [Update Process](update-process.md)
Systemd Service.md
