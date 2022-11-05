# freshrss-stack
Repo to store my FreshRSS stack yaml and create nightly database backups using rclone to Google Drive

Make directories
```
cd ~
mkdir /backups
mkdir -p ~/docker
```
```
cd ~/docker
git clone https://github.com/krimil/freshrss-stack.git freshrss-stack
```

Create nitter.conf file in confs\
Create rclone.conf file in confs

Copy .env-template to .env\
Populate values in .env file

### Install extensions
```
docker exec -it freshrss /bin/sh
cd /config/www/freshrss/extensions
git clone --depth=1 https://github.com/aidistan/freshrss-extensions.git aidistan-extensions
cp -r aidistan-extensions/xExtension-ThemeModeSynchronizer .
```


### Restore From Backups
#### Restore *.conf and .env
```
cd /backups
sudo tar zxvf env_<date>.tar.gz -C ~/docker/freshrss-stack
sudo tar zxvf confs_<date>.tar.gz -C ~/docker/freshrss-stack
```

#### Start FreshRSS Stack
```
docker compose up -d
```

#### Restore FreshRSS config volume
```
cd /backups
docker run --rm --volumes-from freshrss -v /backups:/backups ubuntu bash -c "cd /config && tar zxvf /backups/freshrss_config_<date>.tar.gz --strip 1"
```

#### Restore FreshRSS postgres database
```
docker exec -it daily-backups /bin/sh
apk add postgresql
 
source .env
export PGPASSWORD=$FRESHRSS_DB_PASS

psql -h freshrss-db -U $FRESHRSS_DB_USER $FRESHRSS_DB_NAME -e -c "drop schema public cascade; create schema public"

zcat /backups/freshrss_<date>.sql.gzip | psql -h freshrss-db -d freshrss -U $FRESHRSS_DB_USER
```

#### Restore Invidious postgres database
```
docker exec -it daily-backups /bin/sh
apk add postgresql
 
source .env
export PGPASSWORD=$INVIDIOUS_DB_PASS

psql -h invidious-db -U $INVIDIOUS_DB_USER $INVIDIOUS_DB_NAME -e -c "drop schema public cascade; create schema public"

zcat /backups/invidious_db_<date>.sql.gzip | psql -h invidious-db -d invidious -U $INVIDIOUS_DB_USER

docker compose restart invidious
```
