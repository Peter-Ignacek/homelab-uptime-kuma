Important Notes

The original installation was not a Git repository.
There was no .git directory inside /opt/uptime-kuma, so git pull did not work.

Docker and PM2 are not used.

Update Steps

´´´´cd /opt/uptime-kuma

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
´´´´


