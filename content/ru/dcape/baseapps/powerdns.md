---
title: "powerdns"
date: 2020-01-30T00:38:25+09:00
description: DNS-сервер для поддержки wildcard domain сертификатов
draft: false
weight: 7
---

 Приложение |  [powerdns](https://www.powerdns.com/)
 -- | --
 Docker | [image](https://hub.docker.com/r/psitrax/powerdns)
 Назначение | DNS-сервер для поддержки wildcard domain сертификатов

powerdns - это DNS сервер, который поддерживает

* хранение зон в БД (пример приложения управления зонами - [dcape-dns-config](https://github.com/dopos/dcape-dns-config))
* управление зонами по АПИ (используется в dcape для поддержки wildcard-доменов)