---
title: "DNS setup"
description: "Варианты настройки DNS"
date: 2020-01-28T00:34:13+09:00
draft: false
weight: 3
---

### DNS

Т.к. **dcape** разворачивает несколько независимых сервисов, их имена должны быть прописаны в DNS. Предпочтительным является вариант регистрации wildcard domain, но можно и регистрировать индивидуально.
Пример имен для сервера `srv1.domain.tld`:

* `srv1.domain.tld` - для фронтендов narra, enfist, traefik
* `git.srv1.domain.tld` - для gitea
* `drone.srv1.domain.tld` - для drone
* `port.srv1.domain.tld` - для portainer
* `ns.srv1.domain.tld` - для powerdns

См. также: [DNS setup](README-DNS.md)


## /etc/hosts, для локального использования

```bash
grep -q git.dev.lan /etc/hosts || \
sudo bash -c 'cat >> /etc/hosts <<EOF

127.0.0.1 dev.lan
127.0.0.1 git.dev.lan
127.0.0.1 drone.dev.lan
127.0.0.1 port.dev.lan
127.0.0.1 ns.dev.lan

EOF
'
```

## dnsmasq, для локального использования

```bash
sudo bash -c 'echo "address=/.dev.lan/127.0.0.1" > /etc/NetworkManager/dnsmasq.d/dev.lan.conf'
sudo service network-manager reload
```

## DNS зона, индивидуальная регистрация

```
srv1.domain.tld.        A       192.168.23.10
git.srv1.domain.tld.    A       192.168.23.10
drone.srv1.domain.tld.  A       192.168.23.10
port.srv1.domain.tld.   A       192.168.23.10
ns.srv1.domain.tld.     A       192.168.23.10
```

## DNS зона, wildcard-domain

```
srv1.domain.tld.        A       192.168.23.10
*.srv1.domain.tld.      A       192.168.23.10
```

## DNS зона, wildcard-domain с выделенным сервером для поддержки wildcard сертификатов Let's Encrypt

Для регистрации wildcard сертификатов traefik редактирует зону по АПИ. Чтобы не давать ему доступ к основной DNS-зоне, можно для каждого сервера создать выделенную зону (в примере - `srv1.domain.tld`) и директивой `CNAME` делегировать управление сертификатами этой зоны отдельному серверу (в примере - серверу `ns.srv1.domain.tld`, т.е. локальному DNS). Используемая в **dcape v2** версия traefik это уже поддерживает.

```
srv1.domain.tld.                    A       192.168.23.10
*.srv1.domain.tld.                  A       192.168.23.10

acme-srv1.domain.tld.               NS       ns.srv1.domain.tld
_acme-challenge.srv1.domain.tld.    CNAME    acme-srv1.domain.tld
_acme-challenge.*.srv1.domain.tld.  CNAME    acme-srv1.domain.tld
```

Команда инициализации **dcape** для этого примера:

```bash
make init ACME=wild DNS=wild DCAPE_DOMAIN=srv1.domain.tld \
  TRAEFIK_ACME_EMAIL=admin@domain.tld \
  PDNS_LISTEN=192.168.23.10:53
```
В `PDNS_LISTEN` порт изменен на стандартный (по умолчанию: 54) и задан ip, чтобы не возникало конфликта с локальным резолвером.

См. также: [настройка связки taefik-powerdns](/apps/traefik/Makefile#L98) для `DNS=wild`

---


### Настройка DNS


В DNS зоне для домена your.domain должна быть создана wildcard запись для ip сервера (`.your.domain A ip`)



При установке на локальный компьютер, для доступа к сервисам dcape (cis.dev.lan, port.dev.lan) необходимо настроить wildcard domain *.dev.lan:
[Описание настройки](https://voboghure.com/2020/01/02/enable-wildcard-sub-domain-for-localhost-on-ubuntu-18-04/)

```
sudo bash -c 'echo "address=/.dev.lan/127.0.0.1" > /etc/NetworkManager/dnsmasq.d/dev.lan.conf'
sudo service network-manager reload
```

или можно прописать эти имена в /etc/hosts:
```
sudo bash -c 'echo "127.0.0.1 cis.dev.lan" >> /etc/hosts'
sudo bash -c 'echo "127.0.0.1 port.dev.lan" >> /etc/hosts'
```
но в этом случае придется отдельно прописывать имя для каждого нового сервиса dcape.

* [powerdns](https://www.powerdns.com/) - ([docker](https://store.docker.com/community/images/dopos/powerdns)) DNS-сервер, который хранит описания зон в БД postgresql

