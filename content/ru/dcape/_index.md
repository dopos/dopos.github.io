---
title: "Dcape"
description: "test post index"
date: 2020-01-11T14:09:21+09:00
draft: false
---

DCAPE - это аббревиатура от "**D**ocker **c**omposed **ap**plication **e**nvironment".

[![GitHub Release][1]][2] | ![GitHub code size in bytes][3] | [![GitHub license][4]][5]
--|--|--

[1]: https://img.shields.io/github/release/dopos/dcape.svg
[2]: https://github.com/dopos/dcape/releases
[3]: https://img.shields.io/github/languages/code-size/dopos/dcape.svg
[4]: https://img.shields.io/github/license/dopos/dcape.svg
[5]: LICENSE

[Dcape](https://github.com/dopos/dcape) - это комплект файлов для [make](https://www.gnu.org/software/make/) и [docker-compose](https://docs.docker.com/compose/), который позволяет "по нажатию кнопки" разворачивать и обновлять:

* сторонние приложения из образов docker
* собственные приложения из исходных текстов, размещенных в git

Для поддержки этих процессов на сервере с помощью **dcape** разворачиваются [базовые приложения](dcape/apps/).

## Особенности реализации

* для запуска контейнеров достаточно docker и make (docker-compose запускается в контейнере)
* для настройки приложения достаточно двух файлов - `Makefile` и `docker-compose.yml`
* настройки встроенных приложений размещены в `apps/*/docker-compose.inc.yml`, все эти файлы средствами `make` копируются в `docker-compose.yml` перед запуском `docker-compose`
* файлы `var/apps/*/Makefile` содержат две цели (для адаптированных приложений):
  * `init` - добавление настроек приложения в файл `.env`
  * `apply` - подготовка БД и данных приложения в `var/`
