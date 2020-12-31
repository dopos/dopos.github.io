---
title: "Установка"
description: "Инструкция по установке"
date: 2020-01-28T00:34:13+09:00
draft: false
weight: 4
---

### Исходники

Рекомендуемым способом копирования файлов на сервер является выполнение `git clone`. Это позволяет в будущем

* обновить исходники для получения информации о проверенных новых версиях используемых приложений
* увидеть локальные изменения исходников, если их понадобится сделать


```bash
cd /opt
sudo mkdir dcape && sudo chown $USER dcape
git clone -b v2 --single-branch --depth 1 https://github.com/dopos/dcape.git
cd dcape
```

### Настройка и запуск

### Локальный сервер

Вариант без поддержки SSL, но с установкой gitea. Выполняется в 3 шага, т.к. на шаге 2 необходимо использовать браузер для
* завершения установки gitea
* создания API TOKEN

#### Шаг 1. Подготовка к запуску gitea

```bash
make init DCAPE_DOMAIN=srv1.domain.tld
make apply
make up
```

#### Шаг 2. Запуск и настройка gitea

* открыть `GITEA_URL`, нажать "вход" - откроется страница параметров установки
* ввести логин и пароль учетной записи (логин должен совпадать со значением `DRONE_ADMIN`)
* создать токен (Настройки -> Приложения -> Генерировать токен)

#### Шаг 3. Запуск dcape

```bash
make gitea-setup TOKEN=...
make up
```

`TOKEN` - ключ АПИ gitea, который создается вручную пользователем, имеющим права на создание
* организации, указанной в параметре `NARRA_GITEA_ORG` (если она не создана ранее)
* OAuth2 приложений narra и drone (их CLIENT_ID и CLIENT_KEY будут сохранены в .env).

`TOKEN` используется однократно при выполнении `make gitea-setup` и нигде не сохраняется

См. также: [Issue 22, Автоматизировать первичную настройку Gitea](https://github.com/dopos/dcape/issues/22)

### Примеры make init

```bash {linenos=table,anchorlinenos=true,lineanchors=singlestep}
# сервер для локального использования
make init

# изменение локального порта, по которому будет доступен postgresql (по умолчанию: 5433):
make init PG_PORT_LOCAL=5434

# посмотреть .env без сохранения изменений
make init CFG=tmp$$ DCAPE_VAR=tmp$$-var ACME=wild DNS=wild && less tmp$$ && rm -rf tmp$$*

# сервер без gitea, с wildcard сертификатом
make init ACME=wild DNS=wild DCAPE_DOMAIN=srv1.domain.tld \
  TRAEFIK_ACME_EMAIL=admin@domain.tld \
  PDNS_LISTEN=192.168.23.10:53 \
  NARRA_GITEA_ORG=dcape \
  DRONE_ADMIN=lekovr \
  GITEA=https://it.domain.tld

```

## Использование

* `make up` - старт приложений

После выполнения этой команды все последующее администрирование среды и запущеных сервисов производится в www интерфейсе portainer.
Вместе с тем, в консоли доступны следующие команды:

* `make` - список доступных команд
* `make down` - остановка и удаление всех контейнеров
* `make dc CMD="up -d cis"` - стартовать контейнер заданного приложения (если не запущен)
* `make dc CMD="rm -f -s cis"`- остановить и удалить контейнер
* `make dc CMD="up -d --force-recreate cis"` - пересоздать и стартовать контейнер и его зависимости
* `make db-create NAME=ENFIST` - создать в postgresql пользователя и БД из настроек enfist
* `make db-drop NAME=ENFIST` - удалить пользователя и БД из настроек enfist
* `make apply PG_SOURCE_SUFFIX=-171014` - развернуть проект, используя резервные копии БД, созданные [pg-backup](https://github.com/dopos/dcape-app-pg-backup)
