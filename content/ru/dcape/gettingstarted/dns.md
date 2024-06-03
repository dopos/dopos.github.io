---
title: "Настройка DNS"
description: "Варианты настройки DNS"
date: 2020-01-28T00:34:13+09:00
draft: false
weight: 3
---

Т.к. **dcape** разворачивает несколько независимых сервисов, их имена должны быть зарегистрированы в DNS. Предпочтительным является вариант регистрации wildcard DNS record, но можно и регистрировать индивидуально.
Пример имен для сервера `srv1.domain.tld`:

* `srv1.domain.tld` - для фронтендов narra, enfist, traefik
* `git.srv1.domain.tld` - для gitea
* `drone.srv1.domain.tld` - для drone
* `port.srv1.domain.tld` - для portainer
* `ns.srv1.domain.tld` - для powerdns

## /etc/hosts

Вариант для случая, когда dcape разворачивается и используется на локальном компьютере, в т.ч. если у этого компьютера нет сетевых интерфейсов.

Т.к. сервисы **dcape** общаются между собой, их hostname нельзя привязать к `loopback`- интерфейсу. В качестве ip-адреса можно использовать шлюз подсети dcape, которая задается параметром `DCAPE_SUBNET` (по умолчанию, подсеть - 100.127.0.0/24 и шлюз - 100.127.0.1):

```bash
grep -q " dev.lan" /etc/hosts || \
sudo bash -c 'for n in "" git drone port ns ; do [ "$n" ] && n="$n." ; echo "100.127.0.1 ${n}dev.lan" >> /etc/hosts ; done'
```

## dnsmasq

В случае использования локальной сети, когда известен ip-адрес компьютера с dcape (в примере - 192.168.1.2) и установлен DNS-сервер dnsmasq, можно создать локальный wildcard-domain:

```bash
sudo bash -c 'echo "address=/.dev.lan/192.168.1.2" > /etc/NetworkManager/dnsmasq.d/dev.lan.conf'
sudo service network-manager reload
```

## DNS зона

При наличии доступ к управления DNS-сервером и если хост с **dcape** имеет внешний ip-адрес (в примере - 19.72.10.23), имена могут быть зарегистрированы одним из следующих способов:

### индивидуальная регистрация

```zone.conf
srv1.domain.tld.        A       19.72.10.23
git.srv1.domain.tld.    A       19.72.10.23
cicd.srv1.domain.tld.  A       19.72.10.23
port.srv1.domain.tld.   A       19.72.10.23
ns.srv1.domain.tld.     A       19.72.10.23
```

### wildcard DNS record

```zone.conf
srv1.domain.tld.        A       19.72.10.23
*.srv1.domain.tld.      A       19.72.10.23
```

### wildcard DNS record с делегированием зоны

В процессе регистрации wildcard сертификатов traefik производит изменения в DNS-зоне через АПИ DNS-сервера. Чтобы не давать ему доступ к основной DNS-зоне, можно для каждого сервера создать выделенную зону (в примере - `srv1.domain.tld`) и директивой `CNAME` делегировать управление сертификатами этой зоны отдельному серверу (в примере - серверу `ns.srv1.domain.tld`, т.е. локальному DNS). Используемая в **dcape v2** версия traefik это уже поддерживает.

```zone.conf
srv1.domain.tld.                    A       19.72.10.23
*.srv1.domain.tld.                  A       19.72.10.23

acme-srv1.domain.tld.               NS       ns.srv1.domain.tld
_acme-challenge.srv1.domain.tld.    CNAME    acme-srv1.domain.tld
_acme-challenge.*.srv1.domain.tld.  CNAME    acme-srv1.domain.tld
```

Команда инициализации **dcape** для этого примера:

```bash
make init ACME=wild DNS=wild DCAPE_DOMAIN=srv1.domain.tld \
  TRAEFIK_ACME_EMAIL=admin@domain.tld \
  PDNS_LISTEN=19.72.10.23:53
```

В `PDNS_LISTEN` порт изменен на стандартный (по умолчанию: 54) и задан ip, чтобы не возникало конфликта с локальным резолвером.

См. также: [настройка связки taefik-powerdns](https://github.com/dopos/dcape/blob/v2/apps/traefik/Makefile#L115) для `DNS=wild`
