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
