---
title: "enfist"
date: 2020-01-30T00:38:25+09:00
description: хранилище файлов конфигурации в postgresql с доступом через браузер и АПИ
draft: false
weight: 5
---

 Приложение |  [enfist](https://github.com/apisite/app-enfist)
 -- | --
 Docker | [image](https://hub.docker.com/r/apisite/enfist)
 Назначение | хранилище файлов конфигурации в postgresql с доступом через браузер и АПИ

enfist - это сервис хранения конфигураций приложений, который позволяет редактировать файлы конфигураций

* через web-интерфейс (доступ ограничивается narra)
* через API (для доступа необходимо создать в gitea TOKEN, см [dcape-config-cli](https://github.com/dopos/dcape-config-cli))
