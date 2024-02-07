---
title: "Минимальная конфигурация"
description: "Как развернуть dcape без gitea и DNS"
date: 2024-02-07T00:34:41+09:00
draft: false
enableToc: false
weight: 5
---

Dcape может быть развернут в конфигурации `CLI-only`, с минимальным набором контейнеров

* router (traefik) - реверс-прокси и сертификаты
* db (postgresql) - БД для приложений (не используется dcape)
* manager (portainer) - www-интерфейс для управления контейнерами (не используется dcape)

при этом

* Сертификаты LE обновляются через HTTP-1 (нет поддержки wildcard доменов)
* Доступ в traefik dashboard происходит по фиксированному паролю

Далее описано, как получить такую конфигурацию в текущей версии dcape


```bash
git clone --single-branch --depth 1 https://github.com/dopos/dcape.git
cd dcape
make install DNS=no AUTH_TOKEN=none AUTH_URL=none APPS_ALWAYS=manager \
  DCAPE_DOMAIN=host.example.com \
  ACME=yes TRAEFIK_ACME_EMAIL=admin@example.com
```

Для доступа в traefik dashboard необходимо

* сгенерить пароль

```bash
LOGIN=admin
PASS=$(openssl rand -hex 16; echo)
echo "Login: $LOGIN"
echo "Password: $PASS"
echo -n "HTPASS: "
echo $(htpasswd -nb $LOGIN $PASS) | sed -e s/\\$/\\$\\$/g
```

* добавить в apps/_router/docker-compose.inc.yml после строки
```
      - "traefik.http.routers.dashboard.middlewares=narra"
```
строку
```
      - "traefik.http.middlewares.narra.basicauth.users=<HTPASS>"
```
* выполнить `make up`
