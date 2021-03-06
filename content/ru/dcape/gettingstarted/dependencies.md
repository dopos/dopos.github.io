---
title: "Зависимости"
description: "Приложения, необходимые для установки dcape"
date: 2020-01-28T00:34:13+09:00
draft: false
weight: 2
---

## Зависимости

* linux 64bit с git, make, sed, curl, jq
* [docker](http://docker.io)

`docker-compose` используется в **dcape** в формате [docker-образа](https://hub.docker.com/r/docker/compose/), поэтому отдельной установки не требует.

Пример установки зависимостей:

### docker

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt install docker-ce docker-ce-cli containerd.io
sudo usermod -a -G docker $USER
```

См. также: [Install docker](https://docs.docker.com/engine/install/ubuntu/)

### make, git, sed, curl, jq

```bash
sudo apt install make git sed curl jq
```
