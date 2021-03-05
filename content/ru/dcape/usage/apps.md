---
title: "Приложения"
description: "Адаптация приложения для развертывания в среде dcape"
date: 2020-01-28T00:36:14+09:00
weight: 1
draft: false
---

## Введение

Dcape v2 предназначен для построения gitops (CI/CD) решений, в которых на каждом сервере установлен **dcape** и на одном - web-сервис git (например: gitea), который по факту изменений в репозитории активирует **drone** на присоединенных серверах.

После развёртывания сервисов git/drone, задача dcape уже решена, но возникает возможность добавить в деплой:

* [docker-compose.yml](https://github.com/dopos/dcape/blob/v2/apps/drone/dcape-app/docker-compose.yml)
* [Makefile](https://github.com/dopos/dcape/blob/v2/apps/drone/dcape-app/Makefile)

Эти файлы добавляются в образ `dcape-compose`, поэтому доступны и на хостовой системе и при развёртывании. Для работы с ними в `Makefile` приложения надо добавить директивы:

{{< highlight Makefile "linenos=table,anchorlinenos=true,lineanchors=makefile" >}}
# ------------------------------------------------------------------------------
# Find and include DCAPE/apps/drone/dcape-app/Makefile
DCAPE_COMPOSE  ?= dcape-compose
DCAPE_MAKEFILE ?= $(shell docker inspect -f "{{.Config.Labels.dcape_app_makefile}}" $(DCAPE_COMPOSE))
ifeq ($(shell test -e $(DCAPE_MAKEFILE) && echo -n yes),yes)
  include $(DCAPE_MAKEFILE)
else
  include /opt/dcape-app/Makefile
endif
{{< / highlight >}}
Это позволяет
* не дублировать в make такие цели, как `dc`, `db-create`, `.drone-default`
* директивой `USE_DB=yes` добавлять в .env настройки БД и активировать команды `db-*`
* директивой `ADD_USER=yes` добавлять в .env настройки учетной записи пользователя
* использовать [docker-compose.yml](https://github.com/dopos/dcape/blob/v2/apps/drone/dcape-app/docker-compose.yml) в цели `dc` как основу для перезаписи.

См. также: [Пример использования](https://github.com/dopos/dcape-app-gomodproxy)

## Алгоритм настройки

Первое развёртывание приложения в среде **dcape** состоит из следующих шагов:

* создать репозиторий (или зеркало) в gitea
* активировать репозиторий в drone целевого сервера
* выполнить `git push` (или тест вебхука) - drone произведет попытку развёртывания, в результате которой будет создан файл .env и сохранен в **enfist** с ключом `org--repo_name--branch.sample`
* отредактировать конфигурацию и сохранить ее под именем без суффикса `.sample`
* повторить деплой в drone или тест вебхука - приложение будет развернуто на целевом сервере

После этого push в репозиторий проекта будет приводить к разворачиванию/обновлению приложения в среде **dcape**.

Для развертывания приложения в среде **dcape**, оно должно поддерживать интеграцию с тремя подсистемами:

* [traefik](/dcape/baseapps/traefik)
* [drone](/dcape/baseapps/drone)
* [enfist](/dcape/baseapps/enfist)

Ниже описаны примеры такий интеграции

## Интеграция с traefik

Производится с помощью меток контейнера.

### Пример docker-compose.yml

{{< source "static/samples/docker-compose.yml" >}}

## Интеграция с drone

Производится с помощью файла `.drone.yml`, который размещается в корне репозитория.

### Пример .drone.yml

{{< source "static/samples/.drone.yml" >}}

#### Пояснения по строкам

* [10](/dcape/usage/apps/#static/samples/.drone.yml-10): использовать для развертывания образ `dcape-compose` (создается при установке **dcape**)
* [12](/dcape/usage/apps/#static/samples/.drone.yml-12): интеграция с **enfist**
* [13](/dcape/usage/apps/#static/samples/.drone.yml-13): подготовка окружения и запуск **docker-compose**

### Подготовка окружения



создание персистентного каталога

## Интеграция с enfist

enfist - это сервис хранения файлов конфигурации, которые в dcape имеют имя `.env`. Соответственно, приложение должно уметь работать с конфигурацией, размещенной в таком файле. В части переменных, используемых в `docker-compose.yml`, формат файла должен соответствовать [docker-compose env_file](https://docs.docker.com/compose/compose-file/compose-file-v2/#env_file).

Файл `.env` c переменными для `docker-compose.yml` и другими переменными для запуска приложения не размещается в репозитории, работа с ним при деплое осуществляется командой [setup config](/dcape/usage/apps/#static/samples/.drone.yml-12):

* если в enfist нет конфигурации для текущей комбинации "организация-репозиторий-ветка", выполняется `make .env.sample` (если в репозитории нет .env.sample) и полученный файл сохраняется в enfist с именем "организация-репозиторий-ветка.sample"
* иначе - конфигурация из enfist выгружается в файл .env

Для каждой ветки репозитория создается своя конфигурация запуска.

## Обновление и удаление развернутого приложения

В случае, если префикс (`DCAPE_TAG`) и имя (`APP_TAG`) приложения не изменились - контейнер будет остановлен и удален обновлении приложения. В остальных случаях управление контейнерами и образами может прибыть произведено через portainer

## См. также

* [Актуальный список адаптированных приложений dcape](https://github.com/dopos?q=dcape-app)
