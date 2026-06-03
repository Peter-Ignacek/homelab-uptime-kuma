# Homelab Uptime Kuma

Self-hosted Uptime Kuma monitoring setup for my Proxmox-based homelab infrastructure in Germany and Poland.

## Overview

This repository documents my Uptime Kuma monitoring setup for two homelab locations:

- 🇩🇪 Uptime Kuma DE
- 🇵🇱 Uptime Kuma PL

Both instances run as non-Docker installations inside Proxmox containers.
Some checks are intentionally duplicated across both instances when the same service or dependency needs to be visible from both locations.

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
- [System Service](System-service.md)
- [Duplicati Backup Monitoring](duplicati-backup-monitoring.md)


## Status & Screenshots

Public status pages for both Uptime Kuma instances:

| Instance | Location | Status Page | Screenshot |
|---|---|---|---|
| Uptime Kuma DE | Germany | [Open DE Status Page](https://kuma.de.ignacek.com/status/home) | [Dashboard DE](screenshots/uptime-kuma-dashboard-de.png) |
| Uptime Kuma PL | Poland | [Open PL Status Page](https://kuma.pl.ignacek.com/status/home) | [Dashboard PL](screenshots/uptime-kuma-dashboard-pl.png) |
