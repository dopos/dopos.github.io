---
title: "October 2017"
description: "Dcape v1.0.0-rc1"
date: 2017-08-11T00:10:37+09:00
draft: false
weight: -4
---

## v1.0.0-rc1

Date: 2017-10-22
Github: [dcape](https://github.com/dopos/dcape)
Release: [v1.0.0-rc1, Ready to use](https://github.com/dopos/dcape/releases/tag/v1.0.0-rc1)

### Изменено

* apps/traefik*: в настройки вынесен редирект 80 -> 443
* apps/traefik теперь не совместим по конфигу с apps/traefik-acme, при переключении необходим `make init`

## v0.10

Date: 2017-10-19

### Изменено
* apps/cis: добавлено создание каталогов var/apps, var/log в cis-apply
* apps/cis: изменена версия webtail (0.12)
* apps/enfist: исправлено обновление sql-пакетов enfist,rpc и их текущие версии

### Добавлено

* Файл CHANGELOG.md
* README.md: информация о зависимости (gawk), уточнен блок "Быстрый старт"
* DEPLOY.md: блоки "Информация для разработчика", "Удаление деплоя"
* Makefile: поддержка параметров `PG_PORT_LOCAL`, `CFG_BAK`

### Установка обновления

```bash
git pull
mv .env .env.bak

make init
# Тут будет предупреждение об устаревшей версии webtail - надо изменить на новую в .env

make enfist-apply
# Сообщения "ERROR:  Newest lib version (0.1) loaded already" игнорируем, других ошибок быть не должно

make dc CMD="up -d webtail"
```

