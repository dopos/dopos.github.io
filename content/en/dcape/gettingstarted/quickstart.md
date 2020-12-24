---
title: "Quick Start"
description: "test post"
date: 2020-01-28T00:34:41+09:00
draft: false
weight: 2
---

### Интернет-сервер без gitea

Вариант c поддержкой wildcard-сертификатов SSL, при котором gitea установлена на другом сервере (в примере - `it.domain.tld`) и уже создан `TOKEN`. Настройка и запуск могут быть выполнены одной командой:
```bash
make install ACME=wild DNS=wild DCAPE_DOMAIN=srv1.domain.tld \
  TRAEFIK_ACME_EMAIL=admin@domain.tld \
  NARRA_GITEA_ORG=dcape \
  DRONE_ADMIN=lekovr \
  PDNS_LISTEN=192.168.23.10:53 \
  GITEA=https://it.domain.tld \
  TOKEN=**token_from_gitea**
```
В `PDNS_LISTEN` порт изменен на стандартный (по умолчанию: 54) и задан ip, чтобы не возникало конфликта с локальным резолвером.
