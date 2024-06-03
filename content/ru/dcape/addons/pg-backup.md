---
title: PG Backup
description: Резервное копирование баз postgresql
draft: false
weight: 3
---

> Repo: [dcape-app-pg-backup](https://github.com/dopos/dcape-app-pg-backup)

Postgresql database backup application package for [dcape](https://github.com/dopos/dcape) v3.

## Docker image used

* dcape v1: none (used running dcape_db container)
* dcape v2: internally built image with name stored in ${DCAPE_COMPOSE} drone var
* dcape v3: dcape-compose (cron uses running dcape_db container)

## Requirements

* linux 64bit (git, make, wget, gawk, openssl)
* [docker](http://docker.io)
* [dcape](https://github.com/dopos/dcape) v3
* Git service ([github](https://github.com), [gitea](https://gitea.io) or [gogs](https://gogs.io))

## Usage

* Fork this repo in your Git service
* Setup deploy hook
* Run "Test delivery" (config sample will be created in dcape)
* Edit and save config (enable deploy etc)
* Run "Test delivery" again (app will be installed and started on webhook host)

See also: [Deploy setup](https://github.com/dopos/dcape/blob/master/DEPLOY.md) (in Russian)

## Direct run

If installed via CI/CD
```
docker exec -ti pg-backup-cron-1 bash -c 'cd /root && /etc/periodic/daily/pgbackup.sh'
```

For custom database
```
docker exec -it pg-backup-cron-1 bash -c "make -f /root/Makefile backup DB_NAME=pdns"
```

If installed locally
```
make backup
```

