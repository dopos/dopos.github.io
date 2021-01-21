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

[Dcape](https://github.com/dopos/dcape) - это инструмент создания окружения (среды) для развёртывания [docker](https://www.docker.com/)-приложений по технологии [GitOps](https://www.gitops.tech/). Такое развёртывание состоит из нескольких этапов, для каждого из которых уже существуют opensource-решения (сервисы), но и их, в свою очередь, необходимо сконфигурировать и развернуть. **Dcape**, с помощью [make](https://www.gnu.org/software/make/) и [docker-compose](https://docs.docker.com/compose/), позволяет решить следующие задачи:

* сконфигурировать и развернуть [сервисы](dcape/baseapps/) для всех этапов развёртывания приложений
* сформулировать [правила адаптации приложений](dcape/usage/apps/) к стеку сервисов **dcape**

[Зачем это нужно](/dcape/purpose/)