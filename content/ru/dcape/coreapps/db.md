---
title: db
description: хранение конфигураций всех приложений и размещение баз данных приложений, которым требуется СУБД
draft: false
weight: 2
---

> Repo: [dcape-app-postgres](https://github.com/dopos/dcape-app-postgres)

 Роль в dcape | Сервис | Docker image
 --- | --- | ---
 db | [postgresql](https://www.postgresql.org) / [Citus](https://github.com/citusdata/citus) | [postgres](https://hub.docker.com/_/postgres) / [citus](https://hub.docker.com/r/citusdata/citus)

## Назначение

Размещение баз данных приложений, которым требуется СУБД.

СУБД postgresql используется следующими сервисами dcape:

* gitea
* woodpecker
* enfist
* powerdns

Кроме этого, контейнер доступен для других приложений, развертываемых под dcape.

## Особенности

### Размещение файлов

Постоянные данные контейнера размещаются в каталоге `var/db` в следующих подкаталогах:

* `data` - внутренние файлы postgresql
* `init` - скрипты, которые выполняются при старте контейнера
* `backup` - размещение дампов баз для импорта/экспорта
* `conf.d` - каталог дополнений в конфигурацию, активируется вручную параметром `include_dir` файла `postgresql.conf`
* `shared` - каталог для размещения расширений перед их ручной активацией

### Резервные копии

Для резервного копирования баз данных используется приложение [pg-backup](https://github.com/dopos/dcape-app-pg-backup) которое по заданному в настройках расписанию делает дампы указанных в настройках баз и размещает их в формате `.tgz` в каталоге `var/db/backup`

## Citus

Для использования citisdb вместо postgresql, достаточно в `.env`
указать параметры образа 

```
# Postgresql docker image
DB_IMAGE=citusdata/citus

# Postgresql docker image version
DB_VER=postgres_15
```
и подключить расширение
```
DB_LIB_PREFIX=citus
```

## См. также

### Tuning tools

* [pgconfig](https://www.pgconfig.org/)
* [TimescaleDB tuning tool](https://docs.timescale.com/self-hosted/latest/configuration/timescaledb-tune/)
* [pgtune](https://pgtune.leopard.in.ua/)
* [pgCenter](https://github.com/lesovsky/pgcenter) support (`make pgtop`)

