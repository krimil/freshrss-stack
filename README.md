# freshrss-stack
Repo to store my FreshRSS override yaml and create nightly database backups using rclone to Google Drive

Make directories
```
cd ~
mkdir ../backups
mkdir -p ../freshrss/config
mkdir -p ../freshrss/extensions
mkdir -p ..//nitter-redis
```
Create nitter.conf file in ~/nitter
Create rclone.conf file in ~/rclone

## Clone this repo and copy override.yml to freshrss-overrides folder
```
git clone https://github.com/krimil/freshrss-overrides.git freshrss-overrides
cp freshrss-overrides/docker-compose.override.yml ~/freshrss/
```

Copy .env-template to .env
Populate values in .env file
