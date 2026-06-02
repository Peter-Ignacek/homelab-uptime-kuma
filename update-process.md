Important Notes

The original installation was not a Git repository.
There was no .git directory inside /opt/uptime-kuma, so git pull did not work.

Docker and PM2 are not used.

Update Steps

````cd /opt/uptime-kuma

systemctl stop uptime-kuma

cp -a data data-backup-$(date +%F-%H%M)

apt update
apt install -y git

git init
git remote add origin https://github.com/louislam/uptime-kuma.git
git fetch --tags origin

git checkout -f 2.4.0

npm run setup

systemctl start uptime-kuma
systemctl status uptime-kuma
````

Verification
````

systemctl status uptime-kuma
grep '"version"' /opt/uptime-kuma/package.json
journalctl -u uptime-kuma -n 50 --no-pager
df -h /
````
Expected result:
````
"version": "2.4.0"
````

Backup

Before every update, backup the data folder:
````
cp -a data data-backup-$(date +%F-%H%M)
````

The data folder contains the important database and configuration.


````
### `instances/uptime-kuma-de.md`

# Uptime Kuma DE

## Overview

Uptime Kuma DE monitors the Germany / Mettmann homelab infrastructure.

## System

- Location: Germany
- Platform: Proxmox LXC
- Runtime: Node.js / npm
- Service manager: systemd
- Installation path: `/opt/uptime-kuma`
- Service: `uptime-kuma.service`

## Version

```bash
2.4.0

````
Service Commands
````
systemctl status uptime-kuma
systemctl restart uptime-kuma
journalctl -u uptime-kuma -f
`````
Notes

This instance was updated using the Git-based update method after converting the existing folder into a Git repository.


### `instances/uptime-kuma-pl.md`

````
# Uptime Kuma PL

## Overview

Uptime Kuma PL monitors the Poland homelab infrastructure.

## System

- Location: Poland
- Platform: Proxmox LXC
- Runtime: Node.js / npm
- Service manager: systemd
- Installation path: `/opt/uptime-kuma`
- Service: `uptime-kuma.service`

## Version
2.4.0
````
Service Commands
````
systemctl status uptime-kuma
systemctl restart uptime-kuma
journalctl -u uptime-kuma -f
````


















