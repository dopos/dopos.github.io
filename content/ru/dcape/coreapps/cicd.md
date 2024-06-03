---
title: cicd
description: развёртывание приложений по событиям из gitea
draft: false
weight: 4
---

> Repo: [dcape-app-woodpecker](https://github.com/dopos/dcape-app-woodpecker)

 Роль в dcape | Сервис | Docker images
 --- | --- | ---
 cicd | [Woodpecker CI](https://woodpecker-ci.org/) | [server](https://hub.docker.com/r/woodpeckerci/woodpecker-server), [agent](https://hub.docker.com/r/woodpeckerci/woodpecker-agent)

## Назначение

Деплой приложений при получении webhook от VCS.

## Как это работает

* Приложения (собственные исходные тексты или файлы конфигурации стороннего ПО) размещаются в репозитории на [github.com](https://github.com) или аналогичном сервисе управления git-репозиториями (может использоваться встроенное приложение **gitea**, или другой аналогичный сервис).
* При установке CI/CD указывается адрес сервиса управления git-репозиториями
* В интерфейсе CI/CD активируется нужный git-репозиторий
* При пуше коммита в репозиторий, сервис управления (VCS) активирует webhook всех подключенных экземпляров CI/CD
* Webhook CI/CD клонирует репозиторий и выполняет инструкции из .yml-файла в контексте сервиса, заданного параметром `type`. Этот сервис должен быть запущен вместе с **CI/CD**, в **dcape** в качестве такого сервиса используется [CI/CD-runner-docker](https://github.com/CI/CD-runners/CI/CD-runner-docker)

## Особенности

* если контейнеру надо монтировать каталоги, при активации в **CI/CD** репозиторию надо поставить флаг `Trusted` (это может сделать пользователь, указанный в параметре `CI/CD_ADMIN`, в противном случае этому пользователю будет отправлен запрос на разрешение развертывания)
* при установке CI/CD в dcape создается образ `dcape-compose`, который может быть использован для развёртывания контейнеров
* кроме `docker-compose` и `make`, образ `dcape-compose` [включает](https://github.com/dopos/dcape/blob/v3/Dockerfile)
  * [Makefile.app](https://github.com/dopos/dcape/blob/v3/Makefile.app) для использования в директиве `include` файла `Makefile` адаптируемого приложения (чтобы не дублировать цели) 
  * [docker-compose.app.yml](https://github.com/dopos/dcape/blob/v3/docker-compose.app.yml) для использования в качестве основы для [override](https://docs.docker.com/compose/extends/) в адаптируемом приложении

### Переменные среды CI/CD-docker-runner

* DCAPE_TAG - тег текущей копии dcape, значение из .env
* DCAPE_NET - имя сети dcape, значение из .env
* DCAPE_COMPOSE - имя образа `dcape-compose` для текущей копии dcape (префикс совпадает с `DCAPE_TAG`)
* DCAPE_ROOT - путь к каталогу var (/opt/dcape) хостовой системы, который у выполняющего Ci/CD контейнера примонтирован по этому же пути

## Пример сценария CI/CD

См [сценарий CI/CD для dcape-app-nginx-sample](https://github.com/dopos/dcape-app-nginx-sample/blob/v3/.woodpecker.yml)

## Причины выбора Woodpecker CI

См. [Step-by-step guide to modern, secure and Open-source CI setup](https://devforth.io/blog/step-by-step-guide-to-modern-secure-ci-setup/), 2022.

