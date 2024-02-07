---
title: "Обновление"
description: "Обновление dcape и версий сервисов dcape"
date: 2020-01-28T00:39:06+09:00
weight: 2
draft: false
---

Если dcape был установлен командой `git clone`, для его обновления используется команда `git pull`, после выполнения которой необходимо обновить файл `.env`

## Обновление файла .env {#env-update}

При обновлении проекта возможно появление новых переменных в `.env` файле.
Алгоритм обновления .env с сохранением старых настроек:

```bash
mv .env .env.bak
make init
```

Другой вариант:

```bash
mv .env .env.1019
make init CFG_BAK=.env.1019
```

Все совпадающие значения будут взяты из `.env.bak` (т.е. из старого конфига).
Если изменятся номера версий используемых docker-образов сервисов **dcape**, будут выведены предупреждения.

## Обновление версий сервисов {#version-update}

Для того, чтобы обновить все номера версий используемых docker-образов сервисов **dcape**, сохранив остальные настройки, надо подготовить `.env.bak`, убрав из него номера версий:

```bash
grep -v "_VER=" .env > .env.bak
mv .env .env.pre
make init
```

## Резервирование .env в enfist {#env-backup}

Настройки `dcape/.env` не сохраняются в **enfist** автоматически, но это можно сделать вручную:

```bash
ln -s .env dcape.env
make env-set TAG=dcape
```

## Восстановление сервисов из резервной копии {#restore}

[dcape-app-pg-backup](https://github.com/dopos/dcape-app-pg-backup) предназначен для ежедневного создания резервных копий баз данных, которые сохраняются в `/opt/dcape/var/db/backup`. Кроме такой копии, для восстановления сервиса необходимо перенести соответствующий каталог из `/opt/dcape/var/` (с сохранением владельца файлов).

Пример команды восстановления БД:

```sh
make gitea-apply PG_SOURCE_SUFFIX=-210212
# или
make db-create NAME=GITEA PG_SOURCE_SUFFIX=-210212
```

При восстановлении надо учитывать следующее

1. БД загружается из дампа только при ее создании, т.е. предварительно надо ее удалить, если она есть
2. в копии файлов обычно есть конфиг, в котором задан пароль к БД (пример для gitea - `/opt/dcape/var/gitea/gitea/conf/app.ini`), этот пароль должен совпадать с тем, который указан в `.env`
3. `PG_SOURCE_SUFFIX` используется для формирования имени дампа так: `${DCAPE_DB_DUMP_DEST}/${GITEA_DB_TAG}${PG_SOURCE_SUFFIX}.tgz`, поэтому если имя БД и префикс архива не совпадают, архив надо переименовать