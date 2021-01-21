---
title: "drone"
date: 2020-01-30T00:38:25+09:00
description: развёртывание приложений по событиям из gitea
draft: false
weight: 4
---

 Приложение | [drone](https://github.com/drone)
 -- | --
 Docker | [image](https://hub.docker.com/r/drone/drone)
 Назначение | деплой приложений по событиям из gitea

## Как это работает

* Приложения (собственные исходные тексты или файлы конфигурации стороннего ПО) размещаются в репозитории на [github.com](https://github.com) или аналогичном сервисе управления git-репозиториями (может использоваться встроенное приложение **gitea**, или другой аналогичный сервис).
* При установке drone указывается адрес сервиса управления git-репозиториями
* В интерфейсе drone активируется нужный git-репозиторий
* При пуше коммита в репозиторий, сервис управления активирует webhook всех подключенных экземпляров drone
* Webhook drone клонирует репозиторий и выполняет инструкции из файла .drone.yml в контексте сервиса, заданного параметром `type`. Этот сервис должен быть запущен вместе с **drone**, в **dcape** в качестве такого сервиса используется [drone-runner-docker](https://github.com/drone-runners/drone-runner-docker)

## Особенности

* если контейнеру надо монтировать каталоги, при активации в **drone** репозиторию надо поставить флаг `Trusted` (это может сделать пользователь, указанный в параметре `DRONE_ADMIN`, в противном случае этому пользователю будет отправлен запрос на разрешение развертывания)
* при установке drone в dcape создается образ `dcape-compose`, который может быть использован для развёртывания контейнеров
* кроме `docker-compose` и `make`, образ `dcape-compose` включает
  *  утилиту [setup](https://github.com/dopos/dcape/blob/v2/apps/drone/setup), которая поддерживает 2 команды:
     * `config` - создание или скачивание конфигурации приложения из enfist
     * `root` - создание каталога на хостовой системе и помещение пути к нему в переменную `APP_ROOT`
  * [Makefile](https://github.com/dopos/dcape/blob/v2/apps/drone/dcape-app/Makefile) для использования в директиве `include` файла `Makefile` адаптируемого приложения (чтобы не дублировать цели) 
  * [docker-compose.yml](https://github.com/dopos/dcape/blob/v2/apps/drone/dcape-app/docker-compose.yml) для использования в качестве основы для [override](https://docs.docker.com/compose/extends/) в адаптируемом приложении

### Переменные среды drone-docker-runner

* DCAPE_TAG - тег текущей копии dcape, значение из .env
* DCAPE_NET - имя сети dcape, значение из .env
* DCAPE_COMPOSE - имя образа `dcape-compose` для текущей копии dcape (префикс совпадает с `DCAPE_TAG`)
* DCAPE_ROOT - путь к каталогу var (/opt/dcape/var) хостовой системы, который в **drone-docker-runner** примонтирован по этому же пути

## Пример .drone.yml

{{< source "static/samples/.drone.yml" >}}

См также: [.drone.yml](https://github.com/dopos/dcape-app-nginx-sample/blob/v2/.drone.yml)
