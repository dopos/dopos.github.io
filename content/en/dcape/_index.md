---
title: "Dcape"
description: "test post index"
date: 2020-01-11T14:09:21+09:00
draft: false
---

DCAPE stands for "**D**ocker **c**omposed **ap**plication **e**nvironment".

[![GitHub Release][1]][2] | ![GitHub code size in bytes][3] | [![GitHub license][4]][5]
--|--|--



[1]: https://img.shields.io/github/release/dopos/dcape.svg
[2]: https://github.com/dopos/dcape/releases
[3]: https://img.shields.io/github/languages/code-size/dopos/dcape.svg
[4]: https://img.shields.io/github/license/dopos/dcape.svg
[5]: LICENSE

[Dcape](https://github.com/dopos/dcape) - это комплект файлов для [make](https://www.gnu.org/software/make/) и [docker-compose](https://docs.docker.com/compose/), который ставится на сервер и позволяет "по нажатию кнопки" разворачивать и обновлять:

* сторонние приложения из образов docker
* собственные приложения из исходных текстов, размещенных в git

Для поддержки этих процессов, на сервере с помощью **dcape** разворачиваются базовые приложения:

* [traefik](https://traefik.io/) ([image](https://hub.docker.com/_/traefik/)) - агрегация и проксирование www-сервисов развернутых приложений по заданному имени с поддержкой сертификатов Let's Encrypt
* [postgresql](https://www.postgresql.org) ([image](https://hub.docker.com/_/postgres)) - хранение конфигураций всех приложений и размещение баз данных приложений, которым требуется СУБД
* [gitea](https://gitea.io/) ([image](https://hub.docker.com/r/gitea/gitea)) - git совместимый сервис для работы с репозиториями (если используется несколько серверов, разворачивается только на одном)
* [drone](https://github.com/drone) ([image](https://hub.docker.com/r/drone/drone)) - деплой приложений по событию из gitea
* [narra](https://github.com/dopos/narra) ([image](https://hub.docker.com/r/dopos/narra)) - сервис OAuth2 авторизации для учетных записей gitea, используемый для ограничения доступа к приватным ресурсам
* [enfist](https://github.com/apisite/app-enfist) ([image](https://hub.docker.com/r/apisite/enfist)) - хранилище файлов конфигурации в postgresql с доступом через браузер и АПИ
* [powerdns](https://www.powerdns.com/) ([image](https://hub.docker.com/r/psitrax/powerdns)) - DNS-сервер для поддержки wildcard domain сертификатов
* [portainer](https://portainer.io/) ([image](https://hub.docker.com/r/portainer/portainer/)) - интерфейс к [docker](https://www.docker.com/)

## Особенности реализации

* для запуска контейнеров достаточно docker и make (docker-compose запускается в контейнере)
* для настройки приложения достаточно двух файлов - `Makefile` и `docker-compose.yml`
* настройки встроенных приложений размещены в `apps/*/docker-compose.inc.yml`, все эти файлы средствами `make` копируются в `docker-compose.yml` перед запуском `docker-compose`
* файлы `var/apps/*/Makefile` содержат две цели (для адаптированных приложений):
  * `init` - добавление настроек приложения в файл `.env`
  * `apply` - подготовка БД и данных приложения в `var/`

## Две и более среды dcape на одном сервере

* для второй копии изменить порты в параметрах `TRAEFIK_LISTEN` и `TRAEFIK_LISTEN_SSL`
* изменить параметр `DCAPE_TAG`
