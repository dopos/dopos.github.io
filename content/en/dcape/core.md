---
title: Core
draft: false
weight: 1
---

# dopos/dcape

> Docker-compose application environment

[Dcape](https://github.com/dopos/dcape) is a tool which helps to create environments for [docker](https://www.docker.com/)-applications deployment using [GitOps](https://www.gitops.tech/) technology. **Dcape** based on [make](https://www.gnu.org/software/make/) and [docker-compose](https://docs.docker.com/compose/) and intended to solve the following tasks:

* using `make up` run applications which needs
  * **shared port** (ex. 80)
  * **database**
* using `git push` **deploy applications remotely** on single or several computers
* **manage app configs** through API or web-interface
* **limit** via given user group **access** to used applications interfaces
* support for letsencrypt **wildcard-domains**
* **manage docker objects**

## Applications

For solving of above-mentioned tasks **dcape** uses docker-images of the following applications:

* **shared port**  - [traefik](https://traefik.io/)
  * **database** - [postgresql](https://www.postgresql.org)
* **deploy applications remotely** - [drone](https://github.com/drone) (on every computer) and [gitea](https://gitea.io/) on someone
* **manage app configs** - [enfist](https://github.com/apisite/app-enfist)
* **limit access** - [narra](https://github.com/dopos/narra), [gitea](https://gitea.io/) organization used as user group
* **wildcard-domains** - [powerdns](https://www.powerdns.com/)
* **manage docker objects** - [portainer](https://portainer.io/)

## Documentation

See [dopos.github.io/dcape](https://dopos.github.io/en/dcape)

## Dependensies

* [linux](https://ubuntu.com/download)
* [docker](https://docs.docker.com/engine/install/ubuntu/)
* `sudo apt -y install git make sed curl jq`

## Usage examples

### Deploy app local

Requirements:

* linux computer with docker and **dcape**
* hostnames registered in /etc/hosts or internal DNS (for example - `mysite.dev.test`, `www.mysite.dev.test`) pointing to this computer

#### Static site with nginx

```bash
$ git clone https://github.com/dopos/dcape-app-nginx-sample.git
..
$ cd dcape-app-nginx-sample
$ make init up APP_SITE=mysite.dev.test
..
Creating mysite-dev-lan_www_1 ... done
```

That's all - `http://mysite.dev.test/` and `http://www.mysite.dev.test/` are working.

### Install dcape without gitea

Requirements:

* linux computer with docker and [dependensies](#Dependensies) installed
* DNS records for wildcard-domain `*.srv1.domain.tld`
* Gitea `$AUTH_TOKEN` created

```bash
MY_HOST=${MY_HOST:-srv1.domain.tld}
MY_IP=${MY_IP:-192.168.23.10}
LE_ADMIN=${LE_ADMIN:-admin@domain.tld}
GITEA_URL=${GITEA_URL:-https://git.domain.tld}
GITEA_ORG=${GITEA_ORG:-dcape}
GITEA_USER=${GITEA_USER:-dcapeadmin}

$ git clone https://github.com/dopos/dcape.git
..
$ cd dcape
$ make install ACME=wild DNS=wild DCAPE_DOMAIN=${MY_HOST} \
  TRAEFIK_ACME_EMAIL=${LE_ADMIN} \
  NARRA_GITEA_ORG=${GITEA_ORG} \
  DRONE_ADMIN=${GITEA_USER} \
  PDNS_LISTEN=${MY_IP}:53 \
  GITEA=${GITEA_URL} \
  AUTH_TOKEN=${TOKEN}
..
Running dc command: up -d db powerdns traefik narra enfist drone portainer
Dcape URL: https://srv1.domain.tld
------------------------------------------
Creating network "dcape" with driver "bridge"
Creating dcape_narra_1         ... done
Creating dcape_db_1            ... done
Creating dcape_drone-compose_1 ... done
Creating dcape_portainer_1     ... done
Creating dcape_traefik_1       ... done
Creating dcape_drone-rd_1      ... done
Creating dcape_drone_1         ... done
Creating dcape_powerdns_1      ... done
Creating dcape_enfist_1        ... done

```

That's all - server `srv1.domain.tld` ready for apps deployment, used **dcape** applications are accessible via `https://srv1.domain.tld`.

