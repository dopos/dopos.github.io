---
title: "Dcape"
description: "Docker composed application environment"
date: 2020-01-11T14:09:21+09:00
draft: false
---

> DCAPE - аббревиатура от "**D**ocker **c**omposed **ap**plication **e**nvironment".

[![GitHub Release][1]][2] | ![GitHub code size in bytes][3] | [![GitHub license][4]][5]
--|--|--

[1]: https://img.shields.io/github/release/dopos/dcape.svg
[2]: https://github.com/dopos/dcape/releases
[3]: https://img.shields.io/github/languages/code-size/dopos/dcape.svg
[4]: https://img.shields.io/github/license/dopos/dcape.svg
[5]: LICENSE

[Dcape](https://github.com/dopos/dcape) - это комплект файлов для [make](https://www.gnu.org/software/make/) и [docker-compose](https://docs.docker.com/compose/), который предназначен для решения следующих задач:

* сконфигурировать и развернуть [базовые приложения](dcape/baseapps/), позволяющие в автоматическом режиме производить развертывание docker-приложений
* сформулировать правила оформления [приложений](dcape/usage/apps/), позволяющих развертывание в этой среде
