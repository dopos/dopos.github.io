---
title: "Быстрый старт"
description: "Как развернуть dcape, если уже есть gitea и DNS"
date: 2020-01-28T00:34:41+09:00
draft: false
weight: 1
---

В случае, если уже

* настроен DNS c поддержкой wildcard-record
* есть сервер gitea (в примере - `it.domain.tld`)
* в gitea создан `TOKEN`
* на новом сервере установлены [зависимости](/dcape/gettingstarted/dependencies/)

, установка dcape на этом новом сервере может быть произведена так:

```bash
git clone -b v2 --single-branch --depth 1 https://github.com/dopos/dcape.git
cd dcape
make install ACME=wild DNS=wild DCAPE_DOMAIN=srv1.domain.tld \
  TRAEFIK_ACME_EMAIL=admin@domain.tld \
  NARRA_GITEA_ORG=dcape \
  DRONE_ADMIN=lekovr \
  PDNS_LISTEN=192.168.23.10:53 \
  GITEA=https://it.domain.tld \
  TOKEN=**token_from_gitea**
```

В `PDNS_LISTEN` порт изменен на стандартный (по умолчанию: 54) и задан ip, чтобы не возникало конфликта с локальным резолвером.
