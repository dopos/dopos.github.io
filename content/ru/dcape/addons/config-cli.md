---
title: Config CLI
description: CLI для доступа к файлам конфигурации
draft: false
weight: 4
---

> Repo: [dcape-config-cli](https://github.com/dopos/dcape-config-cli)

Command line interface for [dcape](https://github.com/dopos/dcape) config storage [enfist](https://github.com/apisite/app-enfist).

## Docker image used

* none (used connect to remote dcape config service)

## Requirements

* linux (git, make, curl, jq)

## Setup

* git clone https://github.com/dopos/dcape-config-cli.git
* make .env 
* [write dcape attributes to .env]

`CIS access token` доступен на сервере CIS после авторизации (для авторизации открыть ссылку "Config store" и обновить страницу)

## Usage

* `make ls` - получить список конфигураций на сервере
* `make cat TAG=name` - получить из хранилища конфигурацию для тега `name` и вывести на STDOUT
* `make get TAG=name` - получить из хранилища конфигурацию для тега `name` и сохранить в файл name.env
* `make set TAG=name` - загрузить файл name.env в хранилище с тегом `name` (возвращает `true` если создан новый конфиг)
* `make del TAG=name` - удалить в хранилище тег `name` (возвращает `true` если конфиг удален)

## TODO

* [ ] make push - сохранить все конфиги из текущего каталога на сервер деплоя
* [ ] make pull - выгрузить в текущий каталог все конфиги с сервера деплоя

