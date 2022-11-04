# freshrss-stack
Repo to store my FreshRSS override yaml and create nightly database backups using rclone to Google Drive

Make directories
```
cd ~
mkdir ../backups
mkdir -p ../freshrss/config
mkdir -p ../freshrss/db
mkdir -p ../freshrss/extensions
mkdir -p ../invidious/db
mkdir -p ../nitter-redis
```
Create nitter.conf file in config\
Create rclone.conf file in config

Copy .env-template to .env\
Populate values in .env file
