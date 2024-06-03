---
title: auth
description: развёртывание приложений по событиям из gitea
draft: false
weight: 6
---

> Repo: [dcape-app-narra](https://github.com/dopos/dcape-app-narra)

 Роль в dcape | Сервис | Docker image
 --- | --- | ---
 auth | [narra](https://github.com/dopos/narra) | [ghcr.io/dopos/narra](https://github.com/dopos/narra/pkgs/container/narra)

## Назначение

Сервис OAuth2 авторизации, который разрешает доступ к ресурсу только участникам заданной в настройках организации gitea.

## Ресурсы dcape с ограниченным доступом

* [фронтенд **dcape**](html/private) - список развернутых на сервере приложений и сервисов
* [настройки приложений dcape](https://github.com/dopos/dcape-app-enfist)
* [router](https://github.com/dopos/dcape-app-traefik) dasboard
* статистика [ns](https://github.com/dopos/dcape-app-powerdns)

## Подключение авторизации к приложениям dcape

docker-compose.yml

```yml
services:
  app:
    labels:
      - "traefik.http.routers.${APP_TAG}.middlewares=narra" # Require gitea auth

```

